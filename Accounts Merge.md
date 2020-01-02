# 1070. Accounts Merge
Given a list accounts, each element accounts[i] is a list of strings, where the first element accounts[i][0] is a name, and the rest of the elements are emails representing emails of the account.

Now, we would like to merge these accounts. Two accounts definitely belong to the same person if there is some email that is common to both accounts. Note that even if two accounts have the same name, they may belong to different people as people could have the same name. A person can have any number of accounts initially, but all of their accounts definitely have the same name.

After merging the accounts, return the accounts in the following format: the first element of each account is the name, and the rest of the elements are emails in sorted order. The accounts themselves can be returned in any order.

Example
Example 1:
	Input:
  ```python
	[
		["John", "johnsmith@mail.com", "john00@mail.com"],
		["John", "johnnybravo@mail.com"],
		["John", "johnsmith@mail.com", "john_newyork@mail.com"],
		["Mary", "mary@mail.com"]
	]
```	
	Output: 
  ```python
	[
		["John", 'john00@mail.com', 'john_newyork@mail.com', 'johnsmith@mail.com'],
		["John", "johnnybravo@mail.com"],
		["Mary", "mary@mail.com"]
	]
```
	Explanation: 
	The first and third John's are the same person as they have the common email "johnsmith@mail.com".
	The second John and Mary are different people as none of their email addresses are used by other accounts.

	You could return these lists in any order, for example the answer
	```python
	[
		['Mary', 'mary@mail.com'],
		['John', 'johnnybravo@mail.com'],
		['John', 'john00@mail.com', 'john_newyork@mail.com', 'johnsmith@mail.com']
	]
  ```
	is also acceptable.
  
```python
class Solution:
    def initialize(self, n):
        self.father = {i: i for i in range(n)}
    def accountsMerge(self, accounts):
        self.initialize(len(accounts))
        email_to_id = self.set_email_into_id(accounts)
        
        for email, user_id in email_to_id.items():
            root_user_id = user_id[0]
            for ids in user_id[1:]:
                self.connect(ids, root_user_id)
        
        id_to_email = self.set_id_into_email(accounts)
        merged_email = []
        for user_id, email_set in id_to_email.items():
            merged_email.append([
                accounts[user_id][0],
                *sorted(email_set), 
                #In a function call the single star turns a list into seperate arguments (e.g. zip(*x) is the same as zip(x1,x2,x3) if x=[x1,x2,x3]) and the double star turns a dictionary into seperate keyword arguments (e.g. f(**k) is the same as f(x=my_x, y=my_y) if k = {'x':my_x, 'y':my_y}.
                ])
        return merged_email
        
    
    def set_email_into_id(self, accounts):
        email_to_id = {}
        for i, account in enumerate(accounts):
            for email in account[1:]:
                email_to_id[email] = email_to_id.get(email,[])
                email_to_id[email].append(i)
        return email_to_id
        
    def set_id_into_email(self, accounts):
        id_to_email = {}
        for user_id, account in enumerate(accounts):
            root_user_id = self.find(user_id)
            email_set = id_to_email.get(root_user_id, set())
            for email in account[1:]:
                email_set.add(email)
            id_to_email[root_user_id] = email_set
        return id_to_email
        
    def connect(self, a, b):
        root_a = self.find(a)
        root_b = self.find(b)
        self.father[root_b] = root_a
    
    def find(self, x):
        j = x
        while self.father[j] != j:
            j = self.father[j] #j is the root now
        while x != j:
            fx = self.father[x]
            self.father[x] = j 
            x = fx
        return j
            
```
