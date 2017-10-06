---
layout: post
title: IBM Interview Sample
date: 2017-10-06
tags: Interview IBM
categories: Interview
---

### 

- Define polymorphism and give an example 
    + Polymorphism (多态) is the provision of a single interface to entities of different types.
    + function overloading (can accept different sets of parameters and response differently)
    + Inheritance and subtypes, for example, animal class may have sub-types of cat and dog.

- Given a passage, filter out any passage that is present in the other passages. The passages must also be formatted based on a series of rules (blocks of whitespace should be treated as one whitespace, symbols should be ignored, etc)  


- Write a program that outputs the string representation of numbers from 1 to n. But for multiples of three it should output "Fizz" instead of the number and for the multiples of five output "Buzz". For numbers which are multiples of both three and five output "FizzBuzz".
```c
char** fizzBuzz(int n, int* returnSize) {
    *returnSize = n;
    char buf[11];
    char** re_p = (char**)malloc(sizeof(char*)*n);
    int i = 0;
    for(i=0;i<n;i++)
    {
        if(((i+1)%3==0)&&((i+1)%5==0))
        {
            sprintf(buf,"%s","FizzBuzz");
        }else if((i+1)%3==0){
            sprintf(buf,"%s","Fizz");
        }else if((i+1)%5==0){
            sprintf(buf,"%s","Buzz");
        }else{
            sprintf(buf,"%d",i+1);
        }
        re_p[i]=malloc(sizeof(buf));
        memcpy(re_p[i],buf,strlen(buf)+1);
        memset(buf,"",11);
    }

    return re_p;
}
```
and in python
```py
    def fizzBuzz(self, n):
        """
        :type n: int
        :rtype: List[str]
        """
        results = []
        for i in range(1, n + 1):
            if (i % 3 == 0) and (i % 5) == 0:
                results.append("FizzBuzz")
            elif (i % 3) == 0:
                results.append("Fizz")
            elif (i % 5) == 0:
                results.append("Buzz")
            else:
                results.append(str(i))
        return results
```

https://www.glassdoor.ca/Interview/IBM-Entry-Level-Software-Engineer-Interview-Questions-EI_IE354.0,3_KO4,33_IP2.htm