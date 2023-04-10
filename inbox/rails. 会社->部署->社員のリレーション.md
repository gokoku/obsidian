---
type: note
---

#ruby/rails 

---
2022-08-25  13:30

# rails. 会社->部署->社員のリレーション

![[Pasted image 20220825133231.png]]

```shell
$ rails g model company name:string
$ rails g model department name:string company:references
$ rails g model user name:string department:references
$ rails db:migrate
```



```ruby
models/company.rb

class Campany < ApplicationRecord
  has_many :departments
end


-----------------------------------
models/department.rb

class Department < ApplicationRecord
  belongs_to :campany
  has_many :users
end


-----------------------------------

models/user.rb

class User < ApplicationRecord
  belongs_to :department
end
```

```shell
$ rails c
> company1 = Company.create(name: "ぴーぷる")
> company2 = Company.create(name: "IGR")

> department1 = company1.departments.create(name: "盛岡")

> user1 = department1.users.create(name: "George")


George の会社は?
> user1.department.campany

..."ぴーぶる"...
```

