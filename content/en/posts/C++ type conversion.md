---
title: C++ type conversion 
date: 2019-12-27
lastmod: 2019-12-27 
url:
    /en/tips/type-conversion
tags:
    - Tips  
    - C++
categories:
    - 2019-12
description: "string, char*, const char*, int type conversion"
---

**string, char\*, const char\*, int type conversion**
-----------------------------------------------------

### [string to int/float/double](https://stackoverflow.com/questions/7663709/how-can-i-convert-a-stdstring-to-int)
```cpp
    //older version
    atoi( str.c_str() )
    //------------------------------------------//
    // C++11
    // string -> integer
    int i = std::stoi(s);
    
    // string -> float
    float f = std::stof(s);
    
    // string -> double 
    double d = std::stod(s);
    //------------------------------------------//
    #include <sstream>
    // string -> integer
    std::istringstream ( str ) >> i;
    
    // string -> float
    std::istringstream ( str ) >> f;
    
    // string -> double 
    std::istringstream ( str ) >> d;
    //------------------------------------------//
    // Boost's lexical_cast
    #include <boost/lexical_cast.hpp>
    #include <string>
    
    std::string str;
    
    try {
    	int i = boost::lexical_cast<int>( str.c_str());
    	float f = boost::lexical_cast<int>( str.c_str());
    	double d = boost::lexical_cast<int>( str.c_str());
    	} catch( boost::bad_lexical_cast const&amp;amp;amp; ) {
    		// Error management
    }
```
### [stringto char\* or const char\*](https://stackoverflow.com/questions/7352099/stdstring-to-char)

[https://stackoverflow.com/questions/347949/how-to-convert-a-stdstring-to-const-char-or-char/4152881#4152881](https://stackoverflow.com/questions/347949/how-to-convert-a-stdstring-to-const-char-or-char/4152881#4152881)
```cpp
    char* strtoch(string str){
        char *cstr = new char[str.length() + 1];
        strcpy(cstr, str.c_str());
        return cstr;
    }
    //------------------------------------------//
    std::string x = "hello world";
    const char* p_c_str = x.c_str();
    const char* p_data = x.data();
    const char* p_x0 = &x[0];
    
    char* p_writable_data = x.data(); // for non-const x from C++17 
    char* p_x0_rw = &x[0];  // compiles iff x is not const...
    
    //------------------------------------------//
    std::vector<char> cstr(str.c_str(), str.c_str() + str.size() + 1);
    // or
    std::string str;
    std::vector<char> writable(str.begin(), str.end());
    writable.push_back('\0');
```
### char\* to int
```cpp
    #include <stdlib.h>
    
    int atoi(const char *nptr);
    long atol(const char *nptr);
    long long atoll(const char *nptr);
    long long atoq(const char *nptr);
```
### char to int
```cpp
    // read the value as an ascii code
    char a = 'a';
    // to
    int ia = (int)a; 
    // or
    int ia = a;
    // or
    int num = static_cast<int>(letter); // if letter='a', num=97
    //------------------------------------------//
    //convert the character '0' -> 0, '1' -> 1, etc
    char a = '4';
    int ia = a - '0'; // a - '0' is equivalent to ((int)a) - ((int)'0')
```
### [int to char](https://stackoverflow.com/questions/4629050/convert-an-int-to-ascii-character)
```cpp
    int i = 49;
    char c = i; // c='1' 49=1
    char c = (char)i; // c='1' 49=1
    char c = '0'+i; // c='a' 48+49=97=a
```
### [int to char\*/cstring](http://www.cplusplus.com/reference/cstdlib/itoa/?kw=itoa)
```cpp
    int i = 49;
    char buffer[33];
    char *c = itoa(i,buffer,10);
``
### [int to string](https://stackoverflow.com/questions/5590381/easiest-way-to-convert-int-to-string-in-c)

```cpp
    #include <string> 
    
    int i = 49;
    
    std::string s = std::to_string(i);
    
    // or
    auto s = std::to_string(i);
    
    // or
    char *intStr = itoa(i);
    string str = string(intStr);
    
    // or
    
    #include <sstream>
    
    stringstream ss;
    ss << a;
    string str = ss.str();
    
    // or
    
    #include <sstream>
    
    template <typename T>
    std::string NumberToString ( T Number )
    {
       std::ostringstream ss;
       ss << Number;
       return ss.str();
    }
    // or
    #include <boost/lexical_cast.hpp>
    int num = 4;
    std::string str = boost::lexical_cast<std::string>(num);
```