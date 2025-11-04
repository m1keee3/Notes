Хорошая статья по теме [тут](https://habr.com/ru/companies/otus/articles/527748/)

Горутина описана типом `g`

```go
type g struct {  
	
    stack       stack     
    stackguard0 uintptr 
    stackguard1 uintptr 
	  
    _panic    *_panic  
    _defer    *_defer 
    m         *m      
    sched     gobuf  
    syscallsp uintptr 
    syscallpc uintptr 
    syscallbp uintptr 
    stktopsp  uintptr 
     
    param        unsafe.Pointer  
    atomicstatus atomic.Uint32  
    stackLock    uint32 
    goid         uint64  
    schedlink    guintptr  
    waitsince    int64        
    waitreason   waitReason 
  
    preempt       bool 
    preemptStop   bool 
     
    asyncSafePoint bool  
    paniconfault bool 
    gcscandone   bool    
    throwsplit   bool 
    activeStackChans bool  
    parkingOnChan atomic.Bool  
    
    coroexit     bool 
    raceignore    int8  
    nocgocallback bool  
    tracking      bool  
    trackingSeq   uint8 
    trackingStamp int64  
    runnableTime  int64 
    lockedm       muintptr  
    sig           uint32  
    writebuf      []byte  
    sigcode0      uintptr  
    sigcode1      uintptr  
    sigpc         uintptr  
    parentGoid    uint64         
	gopc          uintptr   
    ancestors     *[]ancestorInfo 
    startpc       uintptr           
    racectx       uintptr  
    waiting       *sudog         
    cgoCtxt       []uintptr
    labels        unsafe.Pointer  
    timer         *timer          
    sleepWhen     int64          
    selectDone    atomic.Uint32
    
    goroutineProfiled goroutineProfileStateHolder
    coroarg *coro
    trace gTraceState
    gcAssistBytes int64
}
```

Структура `g` инициализируется с помощью функции `malg`

```go
//Выделяет новую g со стеком, достаточно большим для stacksize байтов.
func malg(stacksize int32) *g {	
	newg := new(g)      // <--- все начинается здесь 
	if stacksize >= 0 {	
		stacksize = round2(_StackSystem + stacksize)	
		systemstack(func() {		
			newg.stack = stackalloc(uint32(stacksize))	
		})		
		newg.stackguard0 = newg.stack.lo + _StackGuard        
		newg.stackguard1 = ^uintptr(0)       
		...       
		...   
	}    
	return newg
}
```

которая вызывается из [_newproc_](https://github.com/golang/go/blob/f296b7a6f045325a230f77e9bda1470b1270f817/src/runtime/proc.go#L3376) и [_newproc1_](https://github.com/golang/go/blob/f296b7a6f045325a230f77e9bda1470b1270f817/src/runtime/proc.go#L3388).

```go
// Создаем новую g, выполняющую fn с narg байтами аргументов, начинающихся с argp. callerpc - это адрес оператора go, который ее создал. Новая g помещается в очередь g, ожидающих запуска.
func newproc1(fn *funcval, argp unsafe.Pointer, narg int32, callergp *g, callerpc uintptr) {  
	...    
	acquirem() // отключаем вытеснение менее приоритетных задач, потому что оно может удерживать p в локальном var	
	siz := narg    
	siz = (siz + 7) &^ 7  
	...	
	_p_ := _g_.m.p.ptr()	
	newg := gfget(_p_)	
	if newg == nil {	
		newg = malg(_StackMin) // !!! <- здесь горутина и получает значение в 2 КБ
		casgstatus(newg, _Gidle, _Gdead)		
		allgadd(newg) 	
	}  
	...
}   
```

`stack` горутины представляет собой структуру со значениями его начала и конца 

``` go
type stack struct {	
	lo uintptr	
	hi uintptr
}
```

Горутина [**_стартует_**](https://github.com/golang/go/blob/f296b7a6f045325a230f77e9bda1470b1270f817/src/runtime/proc.go#L3410) с минимального размера стека в [**_2 килобайта_**](https://github.com/golang/go/blob/f296b7a6f045325a230f77e9bda1470b1270f817/src/runtime/stack.go#L72), который увеличивается и уменьшается по мере необходимости без риска когда-либо закончиться.

Хотя минимальный размер стека определен как 2048 байтов, рантайм Go также не позволяет горутинам превышать [максимальный размер стека](https://github.com/golang/go/blob/f296b7a6f045325a230f77e9bda1470b1270f817/src/runtime/stack.go#L1031); этот максимум зависит от архитектуры и составляет [1 ГБ для 64-разрядных систем и 250 МБ для 32-разрядных](https://github.com/golang/go/blob/f296b7a6f045325a230f77e9bda1470b1270f817/src/runtime/proc.go#L120) систем.