
# Analyzing AppStore Data to gain Insights about which type of Apps people like the most

Intro - We will See 
Goal - our aim is to help our developers understand what type of apps are likely to attract more users on Google Play and the App Store.

1.In order to open a csv file, We can use csv library. First we import that library/Module using import command then use .extension to access its methods.
  2.We will be using "reader" method to read csv file where it takes file object as the input parameter.
    3.It returns a reader object which we can use to convert into lists for our convinience for Pre-processing the data.


```python
import csv 

#Exploring applestore dataset

apple_store_dataset = open("AppleStore.csv",encoding="utf8")
apple_store_data = csv.reader(apple_store_dataset)
apple_store_data_lists =list(apple_store_data)






```

ApplesStore.csv file contains data about apps from the Apple store as the Name Suggests.It contains information like the Ratings,Genre,No. of Downloads, Minimum age etc.

This Dataset is taken from Kaggle.

The Dataset can be downloaded from the following link:
[Link](https://www.kaggle.com/ramamet4/app-store-apple-data-set-10k-apps/home)

The columns in the dataset are:

|Column Name| Description |
|:--- |:---|
|'id'| App ID|
|'track_name'| App Name|
|'size_bytes'| Size(In Bytes)|
|'currency'| Currency i.e USD,Rupee etc.|
| 'price'|Price according to the Currency|
|'rating_count_tot'|User Rating counts (for all version)|
|'rating_count_ver'|User Rating countsfor current version)|
|'user_rating'|Average User Rating value (for all version)|
|'user_rating_ver'|Average User Rating value (for current version)|
|'ver'|Latest version code|
|'cont_rating'|Content Rating|
|'prime_genre'|Primary Genre|
|'sup_devices.num'|Number of supporting devices|
|'ipadSc_urls.num'|Number of screenshots showed for display|
|'lang.num'|Number of supported languages|
|"Vpp_Lic"|Vpp Device Based Licensing Enabled|


```python
#Function to check rows and columns 
def explore_data(dataset, start, end, rows_and_columns=False):
    dataset_slice = dataset[start:end]    
    for row in dataset_slice:
        print(row)
        print('\n') # adds a new (empty) line after each row

    if rows_and_columns:
        print('Number of rows:', len(dataset))
        print('Number of columns:', len(dataset[0]))
```


```python
explore_data(apple_store_data_lists,0,5, rows_and_columns=True)
```

    ['id', 'track_name', 'size_bytes', 'currency', 'price', 'rating_count_tot', 'rating_count_ver', 'user_rating', 'user_rating_ver', 'ver', 'cont_rating', 'prime_genre', 'sup_devices.num', 'ipadSc_urls.num', 'lang.num', 'vpp_lic']
    
    
    ['284882215', 'Facebook', '389879808', 'USD', '0.0', '2974676', '212', '3.5', '3.5', '95.0', '4+', 'Social Networking', '37', '1', '29', '1']
    
    
    ['389801252', 'Instagram', '113954816', 'USD', '0.0', '2161558', '1289', '4.5', '4.0', '10.23', '12+', 'Photo & Video', '37', '0', '29', '1']
    
    
    ['529479190', 'Clash of Clans', '116476928', 'USD', '0.0', '2130805', '579', '4.5', '4.5', '9.24.12', '9+', 'Games', '38', '5', '18', '1']
    
    
    ['420009108', 'Temple Run', '65921024', 'USD', '0.0', '1724546', '3842', '4.5', '4.0', '1.6.2', '9+', 'Games', '40', '5', '1', '1']
    
    
    Number of rows: 7198
    Number of columns: 16
    


```python
#function to identify duplicate names
def find_duplicates(dataset,index):
    dup_list = []
    unique_list = []
    rows = []
    for row in dataset:
        if row[index] in unique_list:
            dup_list.append(row[index])
            rows.append(row)
        else:
            unique_list.append(row[index])
            
    return dup_list,unique_list
```


```python
duplicates,unique_list = find_duplicates(apple_store_data_lists[1:],1)
print(duplicates)



```

    ['Mannequin Challenge', 'VR Roller Coaster']
    [['1178454060', 'Mannequin Challenge', '59572224', 'USD', '0.0', '105', '58', '4.0', '4.5', '1.0.1', '4+', 'Games', '38', '5', '1', '1'], ['1089824278', 'VR Roller Coaster', '240964608', 'USD', '0.0', '67', '44', '3.5', '4.0', '0.81', '4+', 'Games', '38', '0', '1', '1']]
    


```python
for rows in apple_store_data_lists[1:]:
    app_name = rows[1]
    if app_name == 'Mannequin Challenge':
        print(rows)
print(apple_store_data_lists[0])
```

    ['1173990889', 'Mannequin Challenge', '109705216', 'USD', '0.0', '668', '87', '3.0', '3.0', '1.4', '9+', 'Games', '37', '4', '1', '1']
    ['1178454060', 'Mannequin Challenge', '59572224', 'USD', '0.0', '105', '58', '4.0', '4.5', '1.0.1', '4+', 'Games', '38', '5', '1', '1']
    ['id', 'track_name', 'size_bytes', 'currency', 'price', 'rating_count_tot', 'rating_count_ver', 'user_rating', 'user_rating_ver', 'ver', 'cont_rating', 'prime_genre', 'sup_devices.num', 'ipadSc_urls.num', 'lang.num', 'vpp_lic']
    


```python

def get_index(dataset,index,name):
    res = []
    for row in range(len(dataset)):
        if dataset[row][index] == name:
            res.append(row)
            
    return res[-1]
            
```


```python
row = get_index(apple_store_data_lists,1,'Mannequin Challenge')
print(apple_store_data_lists[row])
```

    ['1178454060', 'Mannequin Challenge', '59572224', 'USD', '0.0', '105', '58', '4.0', '4.5', '1.0.1', '4+', 'Games', '38', '5', '1', '1']
    


```python
del apple_store_data_lists[row]
```


```python
for rows in apple_store_data_lists[1:]:
    app_name = rows[1]
    if app_name == 'VR Roller Coaster':
        print(rows)
print(apple_store_data_lists[0])
```

    ['952877179', 'VR Roller Coaster', '169523200', 'USD', '0.0', '107', '102', '3.5', '3.5', '2.0.0', '4+', 'Games', '37', '5', '1', '1']
    ['1089824278', 'VR Roller Coaster', '240964608', 'USD', '0.0', '67', '44', '3.5', '4.0', '0.81', '4+', 'Games', '38', '0', '1', '1']
    ['id', 'track_name', 'size_bytes', 'currency', 'price', 'rating_count_tot', 'rating_count_ver', 'user_rating', 'user_rating_ver', 'ver', 'cont_rating', 'prime_genre', 'sup_devices.num', 'ipadSc_urls.num', 'lang.num', 'vpp_lic']
    


```python
row = get_index(apple_store_data_lists,1,'VR Roller Coaster')
print(apple_store_data_lists[row])

```

    ['1089824278', 'VR Roller Coaster', '240964608', 'USD', '0.0', '67', '44', '3.5', '4.0', '0.81', '4+', 'Games', '38', '0', '1', '1']
    


```python
del apple_store_data_lists[row]
```


```python
#Dataset is now clean without duolicates now we have to filter English apps and apps that are free so the next few steps will be
# on that process
apple_clean = apple_store_data_lists
```


```python
#Doing sanity check
for rows in apple_store_data_lists[1:]:
    app_name = rows[1]
    if app_name == 'VR Roller Coaster':
        print(rows)
```

    ['952877179', 'VR Roller Coaster', '169523200', 'USD', '0.0', '107', '102', '3.5', '3.5', '2.0.0', '4+', 'Games', '37', '5', '1', '1']
    


```python
for rows in apple_store_data_lists[1:]:
    app_name = rows[1]
    if app_name == 'Mannequin Challenge':
        print(rows)
```

    ['1173990889', 'Mannequin Challenge', '109705216', 'USD', '0.0', '668', '87', '3.0', '3.0', '1.4', '9+', 'Games', '37', '4', '1', '1']
    

As we can see above that only one row is printed so we can confirm that the above dataset is free from duplicates


```python
#function to filter apps which are free
def free_apps(dataset,index):
    free = []
    not_free = []
    for row in dataset:
        if row[index] != '0.0':
            not_free.append(row)
        else:
            free.append(row)
    return free,not_free
            
```


```python
free,not_free = free_apps(apple_clean[1:],4)

print(len(free))
print(len(not_free))
print(len(apple_clean[1:]))
```

    4054
    3141
    7195
    


```python
def find_english_apps(dataset,index):
    english_apps = []
    for row in dataset:
        name = row[index]
        count_non_english = 0
        for char in name:
            if ord(char) > 127 :
                count_non_english +=1
        if count_non_english <= 3:
            english_apps.append(row)
                
    return english_apps
            
            
```


```python
eng_free_apps_apple = find_english_apps(free[1:],1) # Final Apple store Dataset- No Duplicates,All English, All Free
```

#Its hard to verify whether we filtered correctly or not so we save it to a csv and check


```python
with open('english_free_apps.csv','w',encoding="utf8") as file:
    Writer = csv.writer(file)
    Writer.writerows(english_apps)
```

After Checking the CSV file I found some English+ Non English apps:
 429885089,QQ音乐HD,139457536,USD,0.0,224,4,3.5,5.0,5.3.1,4+,Music,24,5,3,1

 593828513,天猫HD,213117952,USD,0.0,198,2,4.5,1.0,5.24.1,17+,Shopping,24,5,2,1
 
 564818797,铁路12306,28961792,USD,0.0,177,0,2.0,0.0,2.80,4+,Travel,38,0,1,1
 
 453691481,飞猪,148888576,USD,0.0,154,0,4.0,0.0,8.2.2,17+,Travel,37,0,1,1
 
 686910560,UC浏览器HD,59791360,USD,0.0,78,2,4.0,5.0,3.0.1.776,17+,Utilities,24,5,1,1
 
 586157728,火车票Pro for 12306,22281216,USD,0.00,76,4,5.0,5.0,7.3.3,4+,Travel,38,4,2,1
 
 299029654,大辞林,210088960,USD,0.0,64,0,4.5,0.0,4.1.1,4+,Reference,37,5,2,1
 
 These are few exceptions that fail our rule (if you find more than three non-english characters you classify it as Non english  so there will be cases where there are excatly three non-english characters or less and these will be classified as English as we have seen above. we wont take these exceptions very seriously so we move on from here.)
 
 



# so far we have removed Non-Free Apps and Non-English Apps and Duplictes from the Apple Store Dataset. Now we will do the same for Google App Store Dataset


```python
google_store_dataset = open("googleplaystore.csv",encoding="utf8")
google_store_data = csv.reader(google_store_dataset)
google_store_data_lists =list(google_store_data)



```

googleplaystore.csv file contains data about apps from the Apple store as the Name Suggests.It contains information like the Ratings,Genre,No. of Downloads, Minimum age etc.

This Dataset is taken from Kaggle.

The Dataset can be downloaded from the following link:
[Link](https://www.kaggle.com/ramamet4/app-store-apple-data-set-10k-apps/home)

The columns in the dataset are:

|Column Name| Description |
|---:|---:|
|'App'| App Name|
|'Category'| Category the App belong to|
|'Rating'| Overall user rating of the app |
|'Reviews'| Number of user reviews for the app|
|"Size"|Size of the app|
|"Installs"|Number of user downloads/installs for the app |
|"Type"| Paid or Free|
|"Price"|Price of the App|
|"Content Rating"|Age group the app is targeted at - Children / Mature 21+ / Adult|
|"Genres"|An app can belong to multiple genres (apart from its main category). For eg, a musical family game will belong to Music, Game, Family genres.|
|"Last Updated"|Date when the app was last updated on Play Store |
|"Current Ver"|Current version of the app available on Play Store|
|"Android Ver"|Min required Android version |




```python
explore_data(google_store_data_lists,0,5, rows_and_columns=True)
```

    ['App', 'Category', 'Rating', 'Reviews', 'Size', 'Installs', 'Type', 'Price', 'Content Rating', 'Genres', 'Last Updated', 'Current Ver', 'Android Ver']
    
    
    ['Photo Editor & Candy Camera & Grid & ScrapBook', 'ART_AND_DESIGN', '4.1', '159', '19M', '10,000+', 'Free', '0', 'Everyone', 'Art & Design', 'January 7, 2018', '1.0.0', '4.0.3 and up']
    
    
    ['Coloring book moana', 'ART_AND_DESIGN', '3.9', '967', '14M', '500,000+', 'Free', '0', 'Everyone', 'Art & Design;Pretend Play', 'January 15, 2018', '2.0.0', '4.0.3 and up']
    
    
    ['U Launcher Lite – FREE Live Cool Themes, Hide Apps', 'ART_AND_DESIGN', '4.7', '87510', '8.7M', '5,000,000+', 'Free', '0', 'Everyone', 'Art & Design', 'August 1, 2018', '1.2.4', '4.0.3 and up']
    
    
    ['Sketch - Draw & Paint', 'ART_AND_DESIGN', '4.5', '215644', '25M', '50,000,000+', 'Free', '0', 'Teen', 'Art & Design', 'June 8, 2018', 'Varies with device', '4.2 and up']
    
    
    Number of rows: 10842
    Number of columns: 13
    


```python
duplicates,unique_lists = find_duplicates(google_store_data_lists[1:],0)
#print(duplicates)
#print(rows)
print("Number of Duplicate Apps "+":"+str(len(duplicates)))
print("Number of Unique Apps "+":"+str(len(unique_lists)))
print("Total No. of Apps : " +str(len(duplicates)) + "+"+ str(len(unique_lists)) + "="+str(len(duplicates)+len(unique_lists)))


```

    Number of Duplicate Apps :1181
    Number of Unique Apps :9660
    Total No. of Apps : 1181+9660=10841
    

# Now the Question is which duplicate should you delete ?

We Can remove apps randomly but we can come up with some better solution.Let Us Print some duplicates to see what we can come up with.


```python
for row in google_store_data_lists[1:]:
    app_name = row[0]
    if app_name == "Telegram":
        print(row)
```

    ['Telegram', 'COMMUNICATION', '4.4', '3128250', 'Varies with device', '100,000,000+', 'Free', '0', 'Mature 17+', 'Communication', 'July 27, 2018', 'Varies with device', 'Varies with device']
    ['Telegram', 'COMMUNICATION', '4.4', '3128509', 'Varies with device', '100,000,000+', 'Free', '0', 'Mature 17+', 'Communication', 'July 27, 2018', 'Varies with device', 'Varies with device']
    ['Telegram', 'COMMUNICATION', '4.4', '3128611', 'Varies with device', '100,000,000+', 'Free', '0', 'Mature 17+', 'Communication', 'July 27, 2018', 'Varies with device', 'Varies with device']
    

 we can see that Number of Reviews(4rth from Left) is highest for the Last row may be we can use this as a testament that this is the latest version as we can't utilize the entry for Latest Version (which is 'Varies with device'). Let's see if this hypothesis can be applied to other apps as well.Let's print some more duplicates.
 


```python
for row in google_store_data_lists[1:]:
    app_name = row[0]
    if app_name == "Instagram":
        print(row)
```

    ['Instagram', 'SOCIAL', '4.5', '66577313', 'Varies with device', '1,000,000,000+', 'Free', '0', 'Teen', 'Social', 'July 31, 2018', 'Varies with device', 'Varies with device']
    ['Instagram', 'SOCIAL', '4.5', '66577446', 'Varies with device', '1,000,000,000+', 'Free', '0', 'Teen', 'Social', 'July 31, 2018', 'Varies with device', 'Varies with device']
    ['Instagram', 'SOCIAL', '4.5', '66577313', 'Varies with device', '1,000,000,000+', 'Free', '0', 'Teen', 'Social', 'July 31, 2018', 'Varies with device', 'Varies with device']
    ['Instagram', 'SOCIAL', '4.5', '66509917', 'Varies with device', '1,000,000,000+', 'Free', '0', 'Teen', 'Social', 'July 31, 2018', 'Varies with device', 'Varies with device']
    

# Viola! We can use the same hypothesis here as well.Let's use this strategy to keep the current version of the app and remove it's duplicates.


```python

def reviews_ft(dataset):
    unique_map = {}
    for app in unique_lists:
        unique_map[app] = 0.0
    for row in dataset:
        No_of_reviews = row[3]
        if No_of_reviews[-1] == 'M':
            No_of_reviews = float(No_of_reviews[:-1]) * 1000000
        else:
            No_of_reviews = float(No_of_reviews)
        app = row[0]
        if app in unique_map:
            if No_of_reviews>unique_map[app] :
                unique_map[app] = No_of_reviews
            
            
            
    return unique_map
    
```


```python
reviews_map = reviews_ft(google_store_data_lists[1:])
print(reviews_map['Telegram'])
print(len(reviews_map))
# we can verify that it is storing the highest review one
```

    3128611.0
    9660
    


```python

def delete_duplicate_google(dataset,reviews_map):
    clean_data = []
    for row in dataset:
        app_name = row[0]
        No_of_reviews = row[3]
        if No_of_reviews[-1] == 'M':
            No_of_reviews = float(No_of_reviews[:-1]) * 1000000
        else:
            No_of_reviews = float(No_of_reviews)
        
        if No_of_reviews == reviews_map[app_name] and (row not in clean_data):
            
            clean_data.append(row)
            
        
    return clean_data
        
```


```python
No_Dups = delete_duplicate_google(google_store_data_lists[1:],reviews_map)
print(len(No_Dups))
```

    9666
    

10841 - 1181 (Total - Duplicates) = 9660. But we can see it is not quite working correctly.Let's try taking one more list to save the app name and add to the result list only if is not there in the list.


```python
clean_data = []
app_list = []

for row in google_store_data_lists[1:]:
    app = row[0]
    N_R = row[3]
    
    if N_R[-1] == "M":
        N_R = float(N_R[:-1])*1000000
    else:
        N_R = float(N_R)
        
    if N_R == reviews_map[app] and (app not in app_list):
        clean_data.append(row)
        app_list.append(app)
        
print(len(clean_data))
```

    9660
    

# Viola! We can see it is giving correct result now.

# Now we will filter only English and Free Apps 


```python
free_clean = []
for row in clean_data:
    Type = row[6]
    Price = float(row[7])
    if Type == "Free" and Price == 0.0:
        free_clean.append(row)
        
print(len(free_clean))
    
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-138-e7e948e60977> in <module>
          2 for row in clean_data:
          3     Type = row[6]
    ----> 4     Price = float(row[7])
          5     if Type == "Free" and Price == 0.0:
          6         free_clean.append(row)
    

    ValueError: could not convert string to float: '$4.99'



```python
free_clean = []
for row in clean_data:
    Type = row[6]
    if Type == "Free":
        free_clean.append(row)
        
print(len(free_clean))
    
```

    8904
    


```python
explore_data(free_clean, 0, 5, rows_and_columns = True)
```

    ['Photo Editor & Candy Camera & Grid & ScrapBook', 'ART_AND_DESIGN', '4.1', '159', '19M', '10,000+', 'Free', '0', 'Everyone', 'Art & Design', 'January 7, 2018', '1.0.0', '4.0.3 and up']
    
    
    ['U Launcher Lite – FREE Live Cool Themes, Hide Apps', 'ART_AND_DESIGN', '4.7', '87510', '8.7M', '5,000,000+', 'Free', '0', 'Everyone', 'Art & Design', 'August 1, 2018', '1.2.4', '4.0.3 and up']
    
    
    ['Sketch - Draw & Paint', 'ART_AND_DESIGN', '4.5', '215644', '25M', '50,000,000+', 'Free', '0', 'Teen', 'Art & Design', 'June 8, 2018', 'Varies with device', '4.2 and up']
    
    
    ['Pixel Draw - Number Art Coloring Book', 'ART_AND_DESIGN', '4.3', '967', '2.8M', '100,000+', 'Free', '0', 'Everyone', 'Art & Design;Creativity', 'June 20, 2018', '1.1', '4.4 and up']
    
    
    ['Paper flowers instructions', 'ART_AND_DESIGN', '4.4', '167', '5.6M', '50,000+', 'Free', '0', 'Everyone', 'Art & Design', 'March 26, 2017', '1.0', '2.3 and up']
    
    
    Number of rows: 8904
    Number of columns: 13
    

# Now we need to filter out English Apps


```python
eng_free_apps_google = find_english_apps(free_clean,0)
print(len(eng_free_apps_google))
print(len(eng_free_apps_apple))
```

    8863
    3219
    

We have 8863 apps from Google Store and 3219 apps from Apple Store

# Most Common Apps by Genre
  
  # Part One
As we mentioned in the introduction, our aim is to determine the kinds of apps that are likely to attract more users because our revenue is highly influenced by the number of people using our apps.

To minimize risks and overhead, our validation strategy for an app idea is comprised of three steps:

Build a minimal Android version of the app, and add it to Google Play.
If the app has a good response from users, we then develop it further.
If the app is profitable after six months, we also build an iOS version of the app and add it to the App Store.
Because our end goal is to add the app on both the App Store and Google Play, we need to find app profiles that are successful on both markets. For instance, a profile that might work well for both markets might be a productivity app that makes use of gamification.

Let's begin the analysis by getting a sense of the most common genres for each market. For this, we'll build a frequency table for the prime_genre column of the App Store data set, and the Genres and Category columns of the Google Play data set.

 # Part Two
We'll build two functions we can use to analyze the frequency tables:

One function to generate frequency tables of Genre and find their percentages
Another function that we can use to display the percentages in a descending order


```python
def genre_ft(dataset,index):
    genre_map ={}
    for row in dataset:
        genre = row[index]
        if genre not in genre_map:
            genre_map[genre] = 1
        else:
            genre_map[genre] +=1
    total_apps = len(dataset) 
    for key in genre_map:
        genre_map[key] /= total_apps
        genre_map[key] *= 100
        
    return genre_map

        
        
```


```python
def disp_ratio_sorted(a_map):
    a_list = []
    for i in a_map:
        a_list.append((i,a_map[i]))
    sorted_list = sorted(a_list, key=lambda x: x[1] , reverse = True)
    final_list = []
    for i in sorted_list:
        final_list.append(i[0] + " : " + str(i[1]))
    return final_list
        
    
        
     
```


```python
#Apple Store
apple_genre_map = genre_ft(eng_free_apps_apple,11)
apple_disp_list = disp_ratio_sorted(genre_map)
for i in apple_disp_list:
    print(i)
```

    Games : 58.1547064305685
    Entertainment : 7.890649269959615
    Photo & Video : 4.970487729108418
    Education : 3.6657347002174587
    Social Networking : 3.2618825722273996
    Shopping : 2.60950605778192
    Utilities : 2.516309412861137
    Sports : 2.1435228331780056
    Music : 2.050326188257223
    Health & Fitness : 2.019260639950295
    Productivity : 1.7396707051879468
    Lifestyle : 1.5843429636533086
    News : 1.3358185771978874
    Travel : 1.2426219322771046
    Finance : 1.1183597390493942
    Weather : 0.8698353525939734
    Food & Drink : 0.8077042559801181
    Reference : 0.5591798695246971
    Business : 0.5281143212177695
    Book : 0.4349176762969867
    Navigation : 0.1863932898415657
    Medical : 0.1863932898415657
    Catalogs : 0.12426219322771047
    

We can see that among the free English apps, more than a half (58.16%) are games. Entertainment apps are close to 8%, followed by photo and video apps, which are close to 5%. Only 3.66% of the apps are designed for education, followed by social networking apps which amount for 3.29% of the apps in our data set.

The general impression is that App Store (at least the part containing free English apps) is dominated by apps that are designed for fun (games, entertainment, photo and video, social networking, sports, music, etc.), while apps with practical purposes (education, shopping, utilities, productivity, lifestyle, etc.) are more rare. However, the fact that fun apps are the most numerous doesn't also imply that they also have the greatest number of users — More apps in the Fun Genre doesn't imply that the there are lot of users who use these. This analysis is just to see how many apps Vs Genres.

Let's continue by examining the Genres and Category columns of the Google Play data set (two columns which seem to be related).


```python
#Google Store
google_genre_map = genre_ft(eng_free_apps_google,1)
google_disp_list = disp_ratio_sorted(google_genre_map)

for i in google_disp_list:
    print(i)
```

    FAMILY : 18.898792733837304
    GAME : 9.725826469592688
    TOOLS : 8.462146000225657
    BUSINESS : 4.592124562789123
    LIFESTYLE : 3.9038700214374367
    PRODUCTIVITY : 3.8925871601038025
    FINANCE : 3.7007785174320205
    MEDICAL : 3.5315355974275078
    SPORTS : 3.396141261423897
    PERSONALIZATION : 3.317161232088458
    COMMUNICATION : 3.2381812027530184
    HEALTH_AND_FITNESS : 3.0802211440821394
    PHOTOGRAPHY : 2.944826808078529
    NEWS_AND_MAGAZINES : 2.798149610741284
    SOCIAL : 2.6627552747376737
    TRAVEL_AND_LOCAL : 2.335552296062281
    SHOPPING : 2.245289405393208
    BOOKS_AND_REFERENCE : 2.1437436533904997
    DATING : 1.8616721200496444
    VIDEO_PLAYERS : 1.7939749520478394
    MAPS_AND_NAVIGATION : 1.399074805370642
    FOOD_AND_DRINK : 1.241114746699763
    EDUCATION : 1.1621347173643235
    ENTERTAINMENT : 0.9590432133589079
    LIBRARIES_AND_DEMO : 0.9364774906916393
    AUTO_AND_VEHICLES : 0.9251946293580051
    HOUSE_AND_HOME : 0.8236488773552973
    WEATHER : 0.8010831546880289
    EVENTS : 0.7108202640189552
    PARENTING : 0.6544059573507841
    ART_AND_DESIGN : 0.6431230960171499
    COMICS : 0.6205573733498815
    BEAUTY : 0.5979916506826132
    

The format of both appstores are different: there are not that many apps designed for fun, and it seems that a good number of apps are designed for practical purposes (family, tools, business, lifestyle, productivity, etc.)

Even so, practical apps seem to have a better representation on Google Play compared to App Store. This picture is also confirmed by the frequency table we see for the Genres column:


```python
google_genre_map = genre_ft(eng_free_apps_google,-4)
google_disp_list = disp_ratio_sorted(google_genre_map)

for i in google_disp_list:
    print(i)
```

    Tools : 8.450863138892023
    Entertainment : 6.070179397495204
    Education : 5.348076272142616
    Business : 4.592124562789123
    Lifestyle : 3.8925871601038025
    Productivity : 3.8925871601038025
    Finance : 3.7007785174320205
    Medical : 3.5315355974275078
    Sports : 3.463838429425702
    Personalization : 3.317161232088458
    Communication : 3.2381812027530184
    Action : 3.102786866749408
    Health & Fitness : 3.0802211440821394
    Photography : 2.944826808078529
    News & Magazines : 2.798149610741284
    Social : 2.6627552747376737
    Travel & Local : 2.324269434728647
    Shopping : 2.245289405393208
    Books & Reference : 2.1437436533904997
    Simulation : 2.042197901387792
    Dating : 1.8616721200496444
    Arcade : 1.8503892587160102
    Video Players & Editors : 1.771409229380571
    Casual : 1.7601263680469368
    Maps & Navigation : 1.399074805370642
    Food & Drink : 1.241114746699763
    Puzzle : 1.128286133363421
    Racing : 0.9928917973598104
    Libraries & Demo : 0.9364774906916393
    Role Playing : 0.9364774906916393
    Auto & Vehicles : 0.9251946293580051
    Strategy : 0.9026289066907368
    House & Home : 0.8236488773552973
    Weather : 0.8010831546880289
    Events : 0.7108202640189552
    Adventure : 0.6769716800180525
    Comics : 0.6092745120162473
    Art & Design : 0.5979916506826132
    Beauty : 0.5979916506826132
    Parenting : 0.4964458986799052
    Card : 0.4513144533453684
    Casino : 0.42874873067809993
    Trivia : 0.4174658693444658
    Educational;Education : 0.3949001466771973
    Board : 0.38361728534356315
    Educational : 0.37233442400992894
    Education;Education : 0.33848584000902626
    Word : 0.25950581067358686
    Casual;Pretend Play : 0.2369400880063184
    Music : 0.20309150400541578
    Entertainment;Music & Video : 0.16924292000451313
    Puzzle;Brain Games : 0.16924292000451313
    Racing;Action & Adventure : 0.16924292000451313
    Casual;Brain Games : 0.1353943360036105
    Casual;Action & Adventure : 0.1353943360036105
    Arcade;Action & Adventure : 0.1241114746699763
    Action;Action & Adventure : 0.10154575200270789
    Educational;Pretend Play : 0.09026289066907367
    Entertainment;Brain Games : 0.07898002933543948
    Simulation;Action & Adventure : 0.07898002933543948
    Board;Brain Games : 0.07898002933543948
    Parenting;Education : 0.07898002933543948
    Art & Design;Creativity : 0.06769716800180525
    Educational;Brain Games : 0.06769716800180525
    Casual;Creativity : 0.06769716800180525
    Parenting;Music & Video : 0.06769716800180525
    Education;Pretend Play : 0.05641430666817105
    Education;Creativity : 0.045131445334536835
    Role Playing;Pretend Play : 0.045131445334536835
    Education;Brain Games : 0.033848584000902626
    Entertainment;Creativity : 0.033848584000902626
    Educational;Creativity : 0.033848584000902626
    Adventure;Action & Adventure : 0.033848584000902626
    Role Playing;Action & Adventure : 0.033848584000902626
    Educational;Action & Adventure : 0.033848584000902626
    Entertainment;Action & Adventure : 0.033848584000902626
    Puzzle;Action & Adventure : 0.033848584000902626
    Education;Action & Adventure : 0.033848584000902626
    Education;Music & Video : 0.033848584000902626
    Casual;Education : 0.022565722667268417
    Music;Music & Video : 0.022565722667268417
    Simulation;Pretend Play : 0.022565722667268417
    Puzzle;Creativity : 0.022565722667268417
    Sports;Action & Adventure : 0.022565722667268417
    Board;Action & Adventure : 0.022565722667268417
    Entertainment;Pretend Play : 0.022565722667268417
    Video Players & Editors;Music & Video : 0.022565722667268417
    Comics;Creativity : 0.011282861333634209
    Lifestyle;Pretend Play : 0.011282861333634209
    Art & Design;Pretend Play : 0.011282861333634209
    Entertainment;Education : 0.011282861333634209
    Arcade;Pretend Play : 0.011282861333634209
    Art & Design;Action & Adventure : 0.011282861333634209
    Strategy;Action & Adventure : 0.011282861333634209
    Music & Audio;Music & Video : 0.011282861333634209
    Health & Fitness;Education : 0.011282861333634209
    Casual;Music & Video : 0.011282861333634209
    Travel & Local;Action & Adventure : 0.011282861333634209
    Tools;Education : 0.011282861333634209
    Parenting;Brain Games : 0.011282861333634209
    Video Players & Editors;Creativity : 0.011282861333634209
    Health & Fitness;Action & Adventure : 0.011282861333634209
    Trivia;Education : 0.011282861333634209
    Lifestyle;Education : 0.011282861333634209
    Card;Action & Adventure : 0.011282861333634209
    Books & Reference;Education : 0.011282861333634209
    Simulation;Education : 0.011282861333634209
    Puzzle;Education : 0.011282861333634209
    Adventure;Education : 0.011282861333634209
    Role Playing;Brain Games : 0.011282861333634209
    Strategy;Education : 0.011282861333634209
    Racing;Pretend Play : 0.011282861333634209
    Communication;Creativity : 0.011282861333634209
    Strategy;Creativity : 0.011282861333634209
    

We can see that Genre column in Apple store is more specific and granular.We are trying to understand the big picture so we will stick with Category Column and skip this Genre Column.
we found that the App Store is dominated by apps designed for fun, while Google Play shows a more balanced landscape of both practical and for-fun apps. Now we'd like to get an idea about the kind of apps that have most users.

# Genre Vs No_of_installs
One way to find out what genres are the most popular (have the most users) is to calculate the average number of installs for each app genre. For the Google Play data set, we can find this information in the Installs column, but this information is missing for the App Store data set. As a workaround, we'll take the total number of user ratings as a proxy.


```python

```
