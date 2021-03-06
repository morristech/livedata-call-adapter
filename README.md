[![Android Arsenal]( https://img.shields.io/badge/Android%20Arsenal-Retrofit2%20LiveData%20Adapter-green.svg?style=flat )]( https://android-arsenal.com/details/1/6743 )
[![Build Status](https://travis-ci.org/leonardoxh/livedata-call-adapter.svg?branch=master)](https://travis-ci.org/leonardoxh/livedata-call-adapter)

LiveData Call Adapter for Retrofit
---
A Retrofit 2 `CallAdapter.Factory` for Android [LiveData](https://developer.android.com/topic/libraries/architecture/livedata.html)

Usage
---
Add `LiveDataCallAdapterFactory` as a `Call` adapter when building your `Retrofit` instance:

```java
Retrofit retrofit = new Retrofit.Builder()
        .baseUrl("https://example.com")
        .addCallAdapterFactory(LiveDataCallAdapterFactory.create())
        .addConverterFactory(LiveDataResponseBodyConverterFactory.create())
        .addConverterFactory(YourConverterFactory.create())
        .build();
```

Your service methods can now use `LiveData` as their return type, but, note that we also have
a `LiveDataResponseBodyConverterFactory` this wrapper is necessary when you have a converter
that touch on the return type like `moshi` or `gson`, to bypass the `Resource<T>` class
and give to the `Converter.Factory` the correct type to serialize.

```java
public interface SuperService {
    @GET("/pimba") LiveData<Resource<Pimba>> getPimba();
    @GET("/pimba") LiveData<Response<Resource<Pimba>>> getPimbas();
}
```

Please note the usage of the `Resource` object, it is required to provide the 
error callback to the `LiveData` object, so when you want verify what is happening 
on your network call for example:

```java
retrofit.create(SuperService.class)
        .getPimba()
        .observe(this, new Observer<Resource<Pimba>>() {
            @Override
            public void onChange(Resource<Pimba> resource) {
                if (resource.isSuccess()) {
                    //doSuccessAction with resource.resource
                } else {
                    //doErrorAction with resource.error
                }
            }
        })
```

Gradle dependency
-----------------
```groovy
dependencies {
    implementation "com.github.leonardoxh:retrofit2-livedata-adapter:1.1.2"
}
```

Inspiration
---
* [Kotlin courtines adapter](https://github.com/JakeWharton/retrofit2-kotlin-coroutines-adapter)
* [Retrofit RXJava2 adapter](https://github.com/square/retrofit)
* [Retrofit Java8 Optional adapter](https://github.com/square/retrofit)

License
---
```
Copyright 2018 Leonardo Rossetto

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
