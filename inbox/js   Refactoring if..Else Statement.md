#js

---
2021-12-15

# Refactoring If..Else Statement


https://towardsdev.com/refactoring-nested-loops-in-js-e6d8a9db2db3


1. Ternary operator  三項式
2. Switch Case : switch 文
3. Short-Circuiting : 

```js
const checkExitMessage = () => {
   return payload && payload[exitMessage] || payload && payload[defaultMessage]
}

const message = checkExitMessage()
```

4. Guard Clauses

```js
const chefkAuthentication = (intentName) => {
   if(!authenticated) return setEvent('Unauthenticated')
   return setEvent(intentName)
   ...
}
```

5. Function Delegation

before
```js
const checkStatus = (entity) => {
  if(!entity) {
    return false
  }
  else if(!checkAuthValue(entity)) {
    setUnauthenticatedMessage(entity)
    retrycount += 1
    return errorMessage()
  } else {
    setAuthenticatedMessage(entity)
    return successMessage()
  }
}
```

after
```js
const checkStatus = (entity) => {
  
	const unauthenticatedUser = () => {
		setUnauthenticatedMessage(entity)
		retryCount += 1
		return errorMessage()
	}
	const authenticatedUser = () => {
		setAuthenticatedMessage(entity)
		return successMessage()
	}
	
	return entity && (checkAuthValue(entity) ? authenticatedUser() : unauthenticatedUser())
}
```

