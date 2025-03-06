
### Свойства

- goroutine-safe
- Хранение элементов, семантика FIFO
- передача данных между горутинами
- блокировка горутин

#### Структура

```
type hchan struct {  
    qcount   uint           // Количество элементов в буфере  
	dataqsiz uint           // Размерность буфера  
	buf      unsafe.Pointer // Ссылка на буфер  
	
	closed   uint32         // Флаг который говорит закрыт ли канал  
	elemtype *_type         // element type  
	
	recvq    waitq          // Очередь на чтение  
	sendq    waitq          // Очередь на запись
	
	recvx    uint           // Номер ячейки в буфера для чтения
	sendx    uint           // Номер ячейки в буфере для записи
}
```