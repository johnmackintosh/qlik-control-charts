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

# p

![image](https://user-images.githubusercontent.com/3278367/70189030-193fee80-16ea-11ea-8b6a-3a510edb1546.png)

//p  

Sum ({$<Type={"P"}>}Numerator/Denominator)

Use a slider (available within Sense as part of the Qlik dashboard extensions, or within QlikView), define a variable
vNumCLPoints, and assign it to the slider.

For SPC, typically between 11 and 25 data points forms a good basis for initial control limits

# Centre Line

![image](https://user-images.githubusercontent.com/3278367/70189104-45f40600-16ea-11ea-87af-26e1feb58f1c.png)


=// Centre line  
if(RowNo()=1,RangeAvg(below(p,0,$(vNumCLPoints))),
If(RowNo()> 1, Above((Centre))))

This expression starts at row 1 and calculates the average of the first n rows - where n is the number of points returned by the slider you have set up above. From row 2 onwards it refers back to the row imediately above.  
This is very much like typing a formula in cell A1 in Excel, then typing =A1 in cell A2, before dragging the formula downwards.
In short - we are locking the centre line to the average of these first n rows.


Now the key to getting this working properly - calculating the standard deviation.

# SD

![image](https://user-images.githubusercontent.com/3278367/70189476-4fca3900-16eb-11ea-8793-725ab79c60db.png)

=//SD  

sqrt((Centre * (1 - Centre))/ sum({$<Type ={"P"}>}[Denominator]))


# Control Limits

There are Upper and Lower Control limits, and Upper and Lower Warning Limits.


![image](https://user-images.githubusercontent.com/3278367/70189839-42fa1500-16ec-11ea-907f-59565acf900f.png)

![image](https://user-images.githubusercontent.com/3278367/70189844-47263280-16ec-11ea-982f-8fc8952e5aff.png)

![image](https://user-images.githubusercontent.com/3278367/70189849-4b525000-16ec-11ea-8c2d-577dbc265ef6.png)

![image](https://user-images.githubusercontent.com/3278367/70189857-50170400-16ec-11ea-88b4-1eb0bd8f90a7.png)


// UCL 
Centre + (3*SD)

//LCL
If(Centre - (3*SD) < 0,0,Centre - (3*SD))
// if LCL < 0, then 0, else LCL

=//UWL
Centre + (2 * SD)

// LWL
If (Centre - (2*SD) < 0, 0, Centre - (2*SD))

For the lower control and warning limits, if the value is less than 0, then we plot 0, else the calculated limits










UCL : 
Centre + (3*SD) 
Centre + (3*SD)




