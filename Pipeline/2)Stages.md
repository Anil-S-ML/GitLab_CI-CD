### Stgaes 
- we can group multiple jobs into stages that run in a defined order 
- multiple jobs in the same stage are executed parallel
- only if all the jobs in the stages got succeded, the pipeline moves on to the next stage 
- if any job in a stage fails , the next stage is not executed and the pipeline fails
- logically group that belongs together , only when all jobs got executed next stage will be executed 