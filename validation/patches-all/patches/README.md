Correct Patch: 
```
               Chart(11): 1, 3, 4, 5, 8, 9, 11, 12, 20, 24, 26

               Closure(14): 2, 14, 21, 38, 46, 57, 62, 63, 73, 86, 92, 93, 104, 118
               
               Lang(13): 6, 10, 16, 24, 26, 29, 33, 38, 45, 46, 55, 57, 59
               
               Math(16): 5, 27, 30, 33, 34, 41, 50, 57, 59, 65, 70, 75, 80, 94, 96, 105
               
               Mockito(2): 29, 38
               
               Time(2): 4, 7
```
Description of Correctness
```
Chart(11): 
1,8,9,11,12,20,24,26: Identical to groundtruth.
3: Method "findBoundsByIteration" resets "copy.minY" and "copy.maxY" to Double.NaN as the groundtruth.
4: The developer patch adds a condition statement "if (r != null)" to the block, the generated patch add a block "if(r==null)continue"
5: The developer patch adds a item into the queue if "this.allowDuplicateXValues" is true, the generated patch also invokes "this.data.add(new XYDataItem(x, y));" to add the item if "this.allowDuplicateXValues" is true.
Closure(14):
14,38,73,86,92,93,104,118: Identical to groundtruth.
2: The developer-wrriten patch assigns the "currentPropertyNames" with a general object to avoid crash when "implicitProto" is null. It is semantically equal to add a return statement here.
21: The developer-wrriten patch removes all the statements of the "ifStatement", while the generated patch adds "if(0!=1)break" to remove the corresponding loop in the block.
46: The generated patch also invokes the corresponding method, "getLeastSupertype", of super class.
57: The developer-wrriten patch adds a condition "target.getType() == Token.STRING", which is also judged in method "isQualifiedName".
62,63: The "charno" is get by the method "getCharno". The range of the return value is -1 ~ sourceExcerpt.length(). Thus, the generated condition "excerpt.equals(LINE)&& 0 <= charno" is equivalent to the developer patches.
Lang(13): 
6,24,26,29,33,38,45,46,57,59,65,70: Identical to groundtruth.
10:
16: The string "str" will be tranformed into lowercase, thus the condition "if (str.startsWith("0x") || str.startsWith("-0x"))" is enough.
55: "RunningState" only can be STATE_RUNNING or STATE_SUSPENDED here. Thus "runningState == STATE_RUNNING" is equal to "runningState != STATE_STATE_SUSPENDED".
Math(16):
5,27,30,34,41,50,57,59,65,70,75,80,94,96,105: Identical to groundtruth.
Mockito(2):
29,38: Identical to groundtruth
Time(2):
4: Identical to groundtruth
7: We justify this patch as plausible patch.
```
