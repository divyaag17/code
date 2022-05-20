# code
from threading import Thread, Event
import time

t_end = time.time() +  1
#while time.time() < t_end:
i = 0
work_1 = Event()
work_2 = Event()

def helper2(w1, w2, i):
   # for i in range(2, 11, 2):
    while time.time() < t_end:
        #time.sleep(1)
        w2.set()  # waking the first thread
        w1.wait()  # waiting for the first thread to get over
       
        w1.clear()  # clearing the flag
        print(i)
        #print(time.time())
        i= i+2

def helper1(w1, w2, i):
    #for i in range(1, 11, 2):
    while time.time() < t_end:
        w2.wait()  # waiting for the second thread to get over
        w2.clear()  # clearing the flag
        
        print(i)
        i = i +2
        #time.sleep(1)
        w1.set()  # used for waking the second thread


# creating threads
thread1 = Thread(target=helper1, args=(work_1, work_2, 0))
thread2 = Thread(target=helper2, args=(work_1, work_2, 1))

thread1.start()
thread2.start()
