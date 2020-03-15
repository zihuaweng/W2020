## Second contribution

### Issue 
[Issue #1050 New feature: findLast(int), findFirst(int), findRandom(int)](https://github.com/realm/realm-java/issues/1050)

The issue we want to solve is #1050. This issue was opened by Sani crysan. He suggested to add findLast(int), findFirst(int) and findRandom(int). The new feature would be useful when the app does not have real data or The database is too large. The findLast(int) and findFirst(int) function are commonly used in database operation.

### Solution
To address this issue, we traced the code into TableQuery.java. In this class, it defines every
query methods here. We can implement new functions here by checking if it’s a valid query and
then call native c++ methods. Or we can just combine the existing query result and implement
them as Sani’s request.
