#### Usage:

Used to preprocess input files on a single machine (because it constructs shared lookup table, 
therefore synchronisation and locking for mutable collection are needed on a multicore-distributed environment).
 
It maps ids to indices (e.g. userId => userIndex, itemId => itemIndex) and saves results to a file with it's lookup table. 

Indices are prefered over ids because linear algebra libraries operate with indices.

Scala time > 45 min (only for item-item matrix)\
C++   time < 5 min  (for both matrices)

Unindexer is used to revert indices to ids after the recommendations are computed.

#### Compile:

using g++:

```
g++ -static -std=c++11 -O3 indexer.cpp -o indexer
g++ -static -std=c++11 -O3 unindexer.cpp -o unindexer
```


or with cmake:

```
mkdir cmake-build-debug
cmake --build cmake-build-debug --target indexer -- -j 4
cmake --build cmake-build-debug --target unindexer -- -j 4
```

#### Run indexer:

```./indexer <item_item_input> <user_item_input>```

Output files will be in the same folder as input with .indexed and .lookup suffix.

#### Output:
```
Header: itemId1,itemId2,a,b,c,d
Processed lines:1000000
...
Processed lines:71000000
Output file with 71930771 lines saved.
Lookup file with 67052 entries saved.
Time: 166s

Header: userId,date,itemId,quantity
Processed lines:1000000
...
Processed lines:29000000
Output file with 29154707 lines saved.
Lookup file with 20998 entries saved.
Time: 50s

Process finished with exit code 0
```

#### Run unindexer:

```./unindexer <recommendations_input> <users_lookup_input> <items_lookup_input>```

<recommendations_input> file must be formated like:

```"userId:itemId1,itemId2,...,itemIdX"```

Output files will be in the same folder as input with .unindexed suffix formated like:

```"userIndex:itemIndex1,itemIndex2,...,itemIndexX"```
 
where k is the number of top k recommendations for a user, so 0 <= X <= k

#### Output:
```
Processed lines:1000
...
Processed lines:2000
Output file with 21225 lines saved.
Time: 4s

Process finished with exit code 0
```
