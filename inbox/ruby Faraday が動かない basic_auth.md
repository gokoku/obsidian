---
type: note
---

#ruby

---
2022-06-02  16:13

# ruby Faraday が動かない basic_auth

API が変わって basic_auth が無くなってた。

```
:basic_auth is not registered on Faraday::Request (Faraday::Error)
```

```ruby
f.request(:basic_auth, ENV['GAROON_BASIC_ID'], ENV['GAROON_BASIC_PW'])
```

を修正して、
 
```ruby
f.request(:authorization, :basic, ENV['GAROON_BASIC_ID'], ENV['GAROON_BASIC_PW'])
```

にするとOK!!!
