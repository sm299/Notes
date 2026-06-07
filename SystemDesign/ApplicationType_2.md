# Type

Before designing you need to ask what kind of application is this<br>
1. Data Intensive Application<br>
2. Compute Intensive Application<br>
   
Now suppose both the system is having same number of users, and using same network and they are facing Latency. <br>
*Latency* -> Taking time to respond<br>

## Data Intensive Application-> <br>
Those who store, gather, move and add data. <br>
### Their speed depends on <br>
1. Database Response<br>
2. Network calls <br>
3. Server <br>
   
### Examples-> <br>
1. Instagram feed <br>
2. Whatsapp <br>
3. Banking Transaction <br>
4. Analytical Dashboard <br>
5. Log Processing System <br>
   
### Worries-> <br>
1. How fast can we read the data <br>
2. How safely we are going to store the data <br>
3. How many users can access it simultaneously <br>
4. What happens if machine dies <br>

### Components to consider-> <br>
1. Databases
2. Caching
3. Replication
4. Sharding
5. Consistency

## Compute Intensive Application-> <br>
Those who store less data but compute more.<br>

### Challenges-> <br>
1. Heavy Computation
2. CPU/GPU bound
   
### Examples-> <br>
1. Image Processing <br>
2. Video Rendering <br>
3. ML Model Training <br>
4. Simulation <br>
5. Cryptography <br>

### Worries-> <br>
1. How fast can we compute <br>
2. Can we parallelize the work <br>
3. How to reduce computational cost <br>
4. Can we use GPU instead of CPU <br>

### Simulator Machine-> <br>
1. Parallel SImulation<br>
2. GPU instead of CPU<br>
3. Cost of Computation<br>
4. Good algorithms

#### Now To Handle an Application Effectively, We Have To Segregate Requirements->
