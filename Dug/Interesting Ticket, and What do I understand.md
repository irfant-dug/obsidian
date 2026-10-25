1. https://downunder.zendesk.com/agent/tickets/370767 - Job fail - tomosolve node/connection issues
2. https://downunder.zendesk.com/agent/tickets/370845 - Houston tank 5-1 (Rocky 9.6 KNL) kernel panics in vastnfs client "dev" build - please rebuild image with latest vastnfs 4.5
3. https://downunder.zendesk.com/agent/tickets/370789 - disk I/O error
4.  https://downunder.zendesk.com/agent/tickets/369244 - Job fails at 100% complete with "marked as finished" error
5. https://downunder.zendesk.com/agent/tickets/370469 - Node failures potentially related to dugwave checkpoint writes
6. https://downunder.zendesk.com/agent/tickets/370164 - Job submission error
```
This gotta be my fault. In IT-7563, I foresee no problem, everything works fine. There's a few flaw with the test.
1. I only test recursive read, no recursive write test
2. I run lfs df before I do write

Apparently, when OST is turned back on, especially for an active client, the connection have to be restarted with lfs df (this will ping the OSTs/MDTs). First lfs df will show endpoint shutdown (effect of non-restarted connection OST).

There's supposed to be some differents betwen conf_param and set_param -p. Yet to be tested but theoretical different as below:
1. 

```
7. a