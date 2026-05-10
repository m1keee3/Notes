
## Первое подключение устройства

  **Типичный flow**

  **На фабрике / при flash'е образа:**                                                                                      
  - На Pi генерируется device_id (UUID) и device_secret (32 рандомных байта). Лежат на SD-карте
  - На корпус клеится QR с device_id + одноразовым pairing_token
  - Сервер при flash'е (или из manifest-файла) добавляет запись в devices(id, secret_hash, pairing_token_hash, claimed_by NULL).                                                                                                     

  **Шаг 1 — Wi-Fi setup (первое включение):**                                                                               
  - Pi видит «Wi-Fi не настроен» → поднимает AP Leafy-XXXX и локальный HTTP-сервер на 192.168.4.1
  - Пользователь в мобилке: «Добавить устройство» → подключается телефоном к этому AP → приложение POST'ит {ssid, password} на Pi → Pi сохраняет, гасит AP, подключается к домашнему Wi-Fi
  - Готовые компоненты: comitup, balena wifi-connect, либо hostapd + dnsmasq руками.       

  **Шаг 2 — Device → Server (автоматически после Wi-Fi):**                                                             
  - Pi подключается к MQTT-брокеру с username=device_id, password=device_secret → брокер (auth-plugin) валидирует через БД
  - Pi публикует leafy/{id}/status = online (retained, LWT).
  - В этот момент Pi уже работоспособен, но **без owner'а** — простаивает, ждёт claim

  **Шаг 3 — Claim (привязка к аккаунту):**                                                                                  
  - Пользователь в приложении сканирует QR → получает device_id + pairing_token
  - Приложение под JWT юзера: POST /api/v1/devices/{id}/claim { pairing_token }
  - Сервер: проверяет pairing_token_hash, ставит claimed_by = user_id, обнуляет pairing_token_hash (одноразовый).       
  - (Опционально для большей защиты: сервер шлёт cmd: ShowPairingCode → Pi выводит 6-значный код на OLED → пользователь вводит его в приложение для подтверждения физического доступа. С QR+токеном это уже избыточно, но если QR можно сфоткать издалека — не лишнее.)                                                                                       

  **Шаг 4 — Привязка растения:**                                                                                            

  - Юзер выбирает растение из каталога → сервер публикует SetPlantConfig в leafy/{id}/cmd → Pi получает, persist'ит локально, начинает автоматику.                               
  
  **Factory reset:**
  - Кнопка на корпусе (long-press) или команда Unclaim от owner'а через приложение.
  - Чистит Wi-Fi, локальный config; сервер обнуляет claimed_by и регенерирует pairing_token.