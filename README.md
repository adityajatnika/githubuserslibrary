# Github Users Library
This repository is made to publish libraries or dependencies that can be used to get data lists and Github user details on Android Studio projects.

## Advantage
✅ Simplify to get github user from API   
✅ Code Eficiency   
✅ No more define API Config    
✅ No more define API service   
✅ No more define Repository Data   

## Developer 
| Nama | NIM |
| --- | --- |
| Prasetya Naufal Rahmandita | 195150401111035 |
| Aditya Akhmad Dinan Jatnika | 195150401111025 |
| Mohammad Aditya Putra | 195150400111054 |
| Ronaldo Simatupang | 195150400111015 |
| Hendika Fashobiqul Galang F. | 195150400111024 |

## System Requirement (Recomendation / Tested)
- Android Studio Dolphin | 2021.3.1 Patch 1
- Kotlin Plugin Version : 213-1.7.20
- Android Gradle Plugin Version : 7.3.1
- Gradle Version : 7.4
- Compile SDK Version : 32 
- Java 8

## Tested on Devices
✅ Samsung Galaxy A03S   
✅ Samsung Galaxy A20S

## How to Use
#### Step 1 : Add the JitPack repository to your build file
Add it in your root build.gradle at the end of repositories:
```gradle
allprojects {
  repositories {
    ...
      maven { url 'https://jitpack.io' }
     }
  }    
```

#### Step 2 : Add the dependency
```gradle
dependencies {
  implementation 'com.github.adityajatnika:githubuserslibrary:Tag'
}
```

#### Step 3 : Choose The Feature
Example :
```kotlin
fun getListUser(query: String = "") {
  isLoading.postValue(true)
  if(query.isEmpty()) {
    val client = ApiConfig.getApiService().getUsers(BuildConfig.API_TOKEN) // <-- 1. Add this function (code import from library) 
    client.enqueue(object : Callback<List<UserResponse>> { // <-- 2. Add some retrofit call in enqueue (UserResponse import from library)
      override fun onResponse(
        call: Call<List<UserResponse>>,
        response: Response<List<UserResponse>>
      ) {
        isLoading.postValue(false)
        if (response.isSuccessful) {
          val responseBody = response.body()
          if (responseBody != null) {
            setListUser(responseBody)
          }
        } else {
          val errorMessage = when (val statusCode = response.code()) {
            ResponseStatus.BAD_REQUEST.stat -> "$statusCode : Bad Request"
            ResponseStatus.FORBIDDEN.stat -> "$statusCode : Forbidden"
            ResponseStatus.NOT_FOUND.stat -> "$statusCode : Not Found"
            else -> "$statusCode"
          }
          Log.e(TAG, errorMessage)
        }
      }
      override fun onFailure(call: Call<List<UserResponse>>, t: Throwable) {
        isLoading.postValue(false)
        stringError.postValue(t.message)
        Log.e(TAG, t.message.toString())
        t.printStackTrace()
      }
    })
  }
}
```
  
That's it! The first time you request a project JitPack checks out the code, builds it and serves the build artifacts (jar, aar).   
If the project doesn't have any GitHub Releases you can use the short commit hash or 'master-SNAPSHOT' as the version.

See also

[Documentation](https://docs.jitpack.io/)  
[Private Repositories](https://jitpack.io/private#auth)   
[Immutable Artifacts](https://docs.jitpack.io/#immutable-artifacts)   

## Features
### GetUsers
In this features provides you to get all data list data users in Github only
##### Syntax :
```kotlin
ApiGithubConfig.ApiService().getUsers(token)
```
##### Output :
| Name | Type | Variabel |
| --- | --- | --- |
| AvatarUrl User | String | avatarUrl |
| ID | Int | id |
| UserName | String | login |
### GetFollower
In this features provides you to get the number of follower based on the selected account
##### Syntax :
```kotlin
ApiGithubConfig.ApiService().getFollowers(token, query)
```
##### Output :
| Name | Type | Variabel |
| --- | --- | --- |
| Number of Follower | Int | followers |
### GetFollowing
In this features provides you to get the number of following based on the selected account 
##### Syntax :
```kotlin
ApiGithubConfig.ApiService().getFollowing(token, query)
```
##### Output :
| Name | Type | Variabel |
| --- | --- | --- |
| Number of Following | Int | following |
### GetDetailUser
In this features provides you to get the detail of account that you choose 
##### Syntax :
```kotlin
ApiGithubConfig.ApiService().getDetailUser(token, query)
```
##### Output :
| Name | Type | Variabel |
| --- | --- | --- |
| Number of Followers | Int | followers |
| AvatarUrl User | String | avatarUrl |
| Number of Following | Int | following |
| Name | String | name |
| Company | String | company |
| Location | String | location |
| ID | Int | id |
| Public Repositories | int | publicRepos |
| UserName | String | login |
## How it Works
- A Configuration API is defined in the library to capture the github base url
- API Service is also included which contains calls to retrofit using endpoints
- Calling data through the RESTFull API, of course, requires a response related to the endpoint
- The response has been defined in the library, there is a UserResponse for calling the Github user list, and a DetailUserResponse for calling Github user details
- The client just needs to call retrofit in the viewmodel and determine its onSuccess and onFailure using the logic in each project.
## Resource
[Github API QUICKGUIDE](https://docs.github.com/en/get-started)
