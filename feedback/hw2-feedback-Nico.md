### ***Project 2 Feedback***

***Nico Van de Bovenkamp***

***

***Quick note:*** You were actually supposed to continue with the admissions data, but this looks great! For assignment 3, we will be moving on to our own data.

**Overall:**  
Great work on this assignment! You made great use of various pandas, and matplotlib functionality. The only direct advice that I have is to keep on practicing and take note of some of the 'tricks' and best practices that you see in class. I know you are comfy with the code, but let me know how the Math study is coming along! Let's setup another time to go back and cover any concepts that you would like.

**Some notes**  
* Balance, Income, Limit, and Rating all look slightly normally distributed, but, they do look a bit skewed right. This is a perfect case to log transform these values! They will make the data log-normal and shift (condense) them to be a bit more normal. ie. try making it `credit['Rating'] = credit['Rating'].apply(np.log)`! By the way, I would recommend that use applys in general.

`apply()` can take any method and apply it across an axis (1 or 0, though usually per column). So you can even write your own functions too! Or a lambda function, ie. `df['gre_log'] = df['gre'].apply(lambda x: 1+np.log(x))`

In general: *applys are good, numpy is good, and straight Python is not so great.**

https://engineering.upside.com/a-beginners-guide-to-optimizing-pandas-code-for-speed-c09ef2c6a4d6

* What would be a solid analysis plan for this dataset? 
