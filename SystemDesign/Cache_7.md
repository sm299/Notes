# Cache

A cache is a temporary storage area that keeps frequently accessed data so it can be retrieved faster, reducing the need to fetch it from the original source (such as a database or server).<br>

Cache , maintained at client side is called Client side cache, same goes for server side cache as well.<br>

1. Cache Hit -> Suppose we have any request coming from front end to backend, and we have cache installed in the system and the request got the response from Cache itself, then this phenomenon will be called Cache hit.<br>
2. Cache Miss -> Now in the above scenario, if we have to hit the db as the cache doesn't have the response for the request, that will be called Cache Miss.<br>
3. TTL(Time To Live) -> It is the duration of the data being there in cache. In cache, data is being stored in key-value pair along the TTL, once TTL is expired then the data gets eliminated from Cache.<br>


### Cache Strategies->
1. RTC(Read Through Cache) -> For this whenever the client will try to reach the db, it has to go via Cache only but only while reading the data.<br>
   a. In case of Cache Hit-> Cache will give the data to the Backend, when Client asks for data.<br>
   b. In case of Cache Miss-> Cache won't have data , so Cache will ask DB for the data when Backend asks for it, and will give the same to Backend.<br>
2. WTC(Write Through Cache) -> This strategy is only for writing. So whenever client will go to write data in db, it will first write in Cache then in DB. Mainly used in Stock Market application as always the latest data will be needed there.<br>
3. WAC(Write Around Cache) -> For this while writing ,client will directly write in DB. But while reading it will first go to Cache for data, then to DB if not found, but in case of Cache miss, while reading data from db, it will first update cache, then will give the value to Backend. Ex: Twitter/X<br>
4. WBC(Write Back Cache) -> Here, whenever client wants to write anything, it will be written in Cache, later one async process will update the same data in db. While reading, mostly we get Cache hit if not, will look for data in db and update cache. Ex: Swiggy/Zomato. <br>
   

### Cache Eviction Policies ->
1. LRU(Least Recent Used) -> Delete the data which has been fetched least at recent times. Suppose Apple launched IPhone 17 max, that time Iphone 11 is not in trend, then those data can be deleted given the users are not interested in that and IpHone 11 is not getting that much hit.<br>
2. MRU(Most Recent Used) -> Delete the data which has been fetched highest at recent times. Suppose we are watching a youtube video of 1 hr and as youtube videos get loaded in chunks, suppose 15 mins in this case, so when we reach at 50 mins, it's unlikely that we come back to the first three parts, or first two parts may be, so it gets delete from cache. <br>
3. LFU(Least Frequently Used) -> Suppose I searches for gadgets and clothes very frequently but not for plants that much. Maybe once a year, so when the shopping sites will evict my data, pplant will be removed first.<br>
4. FIFO (First in First out) <br>
5. LIFO (Last in First out) <br>