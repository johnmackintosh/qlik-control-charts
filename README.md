# qlik-control-charts
materials for and from my presentation to the  Qlik Healthcare Dev Group September Gathering 2017-09-14


- See attached powerpoint for examples of these charts
- Most charts are built in a similar manner - but the method of calculating SD varies for each one
- Don't use the build in standard deviation function


## P Chart

![image](https://user-images.githubusercontent.com/3278367/70188539-e0534a00-16e8-11ea-95fb-068fee4d5014.png)



In QlikView your final chart properties will look something like this.  
These chart expressions also work in Sense.

![image](https://user-images.githubusercontent.com/3278367/70188300-396eae00-16e8-11ea-8547-c1cf92175e39.png)

Assuming you are using the sample data, we will need to specify the 'Type' variable in the set analysis.
This may not be necessary with your real life data

![image](https://user-images.githubusercontent.com/3278367/70189030-193fee80-16ea-11ea-8b6a-3a510edb1546.png)

//p  

Sum ({$<Type={"P"}>}Numerator/Denominator)

Use a slider (available within Sense as part of the Qlik dashboard extensions, or within QlikView), define a variable
vNumCLPoints, and assign it to the slider.

For SPC, typically between 11 and 25 data points forms a good basis for initial control limits


![image](https://user-images.githubusercontent.com/3278367/70189104-45f40600-16ea-11ea-87af-26e1feb58f1c.png)


=// Centre line  
if(RowNo()=1,RangeAvg(below(p,0,$(vNumCLPoints))),
If(RowNo()> 1, Above((Centre))))

This expression calculates the average of the first n rows - where n is the number of points returned by the slider you have set up above.



