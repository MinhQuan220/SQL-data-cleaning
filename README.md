# SQL-data-cleaning

This is an educational project on data cleaning and preparation using SQL. The original database in CSV format is located in the file club_member_info.csv. Here, we will explore the steps that need to be applied to obtain a cleansed version of the dataset.

## Data Introduction
```SQL
SELECT * FROM club_member_info cmi LIMIT 10;
```
The result:
|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|
|addie lush|40|married|alush0@shutterfly.com|254-389-8708|3226 Eastlawn Pass,Temple,Texas|Assistant Professor|7/31/2013|
|      ROCK CRADICK|46|married|rcradick1@newsvine.com|910-566-2007|4 Harbort Avenue,Fayetteville,North Carolina|Programmer III|5/27/2018|
|Sydel Sharvell|46|divorced|ssharvell2@amazon.co.jp|702-187-8715|4 School Place,Las Vegas,Nevada|Budget/Accounting Analyst I|10/6/2017|
|Constantin de la cruz|35||co3@bloglines.com|402-688-7162|6 Monument Crossing,Omaha,Nebraska|Desktop Support Technician|10/20/2015|
|  Gaylor Redhole|38|married|gredhole4@japanpost.jp|917-394-6001|88 Cherokee Pass,New York City,New York|Legal Assistant|5/29/2019|
|Wanda del mar       |44|single|wkunzel5@slideshare.net|937-467-6942|10864 Buhler Plaza,Hamilton,Ohio|Human Resources Assistant IV|3/24/2015|
|Joann Kenealy|41|married|jkenealy6@bloomberg.com|513-726-9885|733 Hagan Parkway,Cincinnati,Ohio|Accountant IV|4/17/2013|
|   Joete Cudiff|51|divorced|jcudiff7@ycombinator.com|616-617-0965|975 Dwight Plaza,Grand Rapids,Michigan|Research Nurse|11/16/2014|
|mendie alexandrescu|46|single|malexandrescu8@state.gov|504-918-4753|34 Delladonna Terrace,New Orleans,Louisiana|Systems Administrator III|3/12/1921|
| fey kloss|52|married|fkloss9@godaddy.com|808-177-0318|8976 Jackson Park,Honolulu,Hawaii|Chemical Engineer|11/5/2014|

## Some issues with the data
'full_name' column
- Inconsistent letter case
- Leading and trailing whitespaces

'age' column
- Age out of realistic range

'martial_status' column
- Spelling errors
- Data has blank cells

'phone' colum
- incorrect data
- Data has blank cells

'job_title' colum
- Data has blank cells
  
'membership_date' column
- Year out of realistic range

## Step 1: Copy table
### Create new table for cleaning
```SQL
-- club_member_info definition
CREATE TABLE club_member_info_cleaned (
	full_name VARCHAR(50),
	age INTEGER,
	martial_status VARCHAR(50),
	email VARCHAR(50),
	phone VARCHAR(50),
	full_address VARCHAR(50),
	job_title VARCHAR(50),
	membership_date VARCHAR(50)
);
```
### Copy all values from original table
```SQL
INSERT INTO club_member_info_cleaned
SELECT * FROM club_member_info;
```
## Step 2: Customize and standardize the 'full_name' column data
### Remove leading and trailing spaces in column 'full_name'
```SQL
UPDATE club_member_info_cleaned  SET full_name = TRIM(full_name);
```
### Normalize data: Convert column 'full_name' to all uppercase.
```SQL
UPDATE club_member_info_cleaned  SET full_name = UPPER(full_name);
```
The result:
|full_name|
|---------|
|ADDIE LUSH|
|ROCK CRADICK|
|SYDEL SHARVELL|
|CONSTANTIN DE LA CRUZ|
|GAYLOR REDHOLE|
|WANDA DEL MAR|
|JOANN KENEALY|
|JOETE CUDIFF|
|MENDIE ALEXANDRESCU|
|FEY KLOSS|

## Step 3: Customize and standardize the 'age' column data
Hypothesis: when entering data, it was entered incorrectly because the last number was double-clicked when entering
### Correct Unrealistic Age Values ​​in the Column
```SQL
UPDATE club_member_info_cleaned
SET age = SUBSTR(age, 1,2)
WHERE LENGTH(age) = 3;
```
The result:
```SQL
SELECT DISTINCT age  FROM club_member_info_cleaned;
```
|age|
|---|
|18|
|19|
|20|
|21|
|22|
|23|
|24|
|25|
|26|
|27|
|28|
|29|
|30|
|31|
|32|
|33|
|34|
|35|
|36|
|37|
|38|
|39|
|40|
|41|
|42|
|43|
|44|
|45|
|46|
|47|
|48|
|49|
|50|
|51|
|52|
|53|
|54|
|55|
|56|
|57|
|58|
|59|
|60|
|61|
|62|
|63|
|64|
|65|
|66|
|67|
|68|
||
## Step 4: Customize and standardize the 'marital_status' column data
### Correct Spelling Errors in the 'marital_status' Column
```SQL
UPDATE club_member_info_cleaned 
SET martial_status = 'divorced'
WHERE martial_status = 'divored';
```
### Replace missing values ​​in column 'marital_status' with 'unknown'
```SQL
UPDATE club_member_info_cleaned 
SET martial_status = 'unknown'
WHERE TRIM(martial_status) = '';
```
The result:
```SQL
SELECT DISTINCT martial_status  FROM club_member_info_cleaned cmic;
```
|martial_status|
|--------------|
|married|
|divorced|
|unknown|
|single|
## Step 5: Handle Missing and Incomplete Data in the 'phone' column
```SQL
UPDATE club_member_info_cleaned  
SET phone = 'unknown'
WHERE LENGTH(phone) != 12
	OR TRIM(phone) = '';
```
The result:
```SQL
SELECT *
FROM club_member_info_cleaned cmic 
WHERE LENGTH(phone) != 12;
```
|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|
|ANNALIESE ETOILE|52|married|aetoile2w@artisteer.com|unknown|43 Nova Circle,Garden Grove,California|Financial Advisor|10/1/1912|
|AJAY DEGENHARDT|53|married|adegenhardtec@chronoengine.com|unknown|45479 Mayer Street,Newport Beach,California|Help Desk Technician|10/23/2019|
|SMITTY BULMER|52|divorced|sbulmergm@addthis.com|unknown|23370 Forest Dale Street,Pittsburgh,Pennsylvania|VP Marketing|9/25/2017|
|FRANCISCO PISER|31|married|fpiserkc@tamu.edu|unknown|0 Northport Center,Alexandria,Virginia|Marketing Manager|8/4/2012|
|MARION HAWLER|55|married|mhawler1p@pinterest.com|unknown|60701 Crest Line Drive,Fresno,California|Teacher|1/21/2022|
|SHIRLINE ESHMADE|41|married|seshmade9h@ed.gov|unknown|331 Bellgrove Hill,Richmond,Virginia|Quality Engineer|10/8/2016|
|CIRILLO BARTLOMIEJCZYK|31|divorced|cbartlomiejczykdj@networksolutions.com|unknown|4426 Rigney Alley,Henderson,Nevada|Analog Circuit Design manager|9/2/2015|
|GAIL CUTLER|26|married|gcutlerf2@auda.org.au|unknown|242 Homewood Junction,Pittsburgh,Pennsylvania|Community Outreach Specialist|11/27/2019|
|BLONDIE RICCARDO|66|married|briccardofn@about.com|unknown|19 Mayer Drive,Chattanooga,Tennessee|Financial Advisor|9/8/2020|
|WILFRID DANILOV|63|married|wdanilovix@is.gd|unknown|5208 Tennyson Road,Dallas,Texas|Account Executive|5/16/2015|
|LANNIE CLEMMEY|42|married|lclemmeykj@redcross.org|unknown|572 Golf Junction,Mountain View,California|VP Sales|4/8/2016|
|SHARLA MACIOCIA|65|single|smaciocian3@adobe.com|unknown|141 Badeau Plaza,Fresno,California|Staff Accountant IV|6/25/2018|
|JOELLA SURR|32|married|jsurrpo@meetup.com|unknown|775 Boyd Avenue,Houston,Texas|Nurse Practicioner|3/18/2017|
|CLAYBORNE PENELLI|23|single|cpenellirf@apple.com|unknown|01412 Dunning Place,Washington,District of Columbia|Web Developer III|12/24/2021|

## Step 6: Handle Missing Data in the 'job_title' column
### Check missing data rows
```SQL
SELECT * FROM club_member_info_cleaned cmic 
WHERE TRIM(job_title) = '';
```
### Change rows with missing data to unknown
```SQL
UPDATE club_member_info_cleaned
SET job_title = 'unknown'
WHERE TRIM(job_title) = '';
```
The result:
|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|
|EPHREM BRAUNTER|26|married|ebraunter15@state.gov|859-422-6180|74155 Dexter Point,Lexington,Kentucky|unknown|11/20/2016|
|LESLY GYLES|33|divorced|lgyles30@java.com|316-673-6530|5472 Farmco Avenue,Wichita,Kansas|unknown|8/19/2021|
|MIKOL CONNAR|27|single|mconnar5f@mtv.com|509-540-4865|6 Express Way,Spokane,Washington|unknown|4/9/2017|
|BENNY CASALI|36|married|bcasali7i@china.com.cn|203-729-8147|45459 Schlimgen Road,New Haven,Connecticut|unknown|3/7/2018|
|PATRIC ALESHKOV|54|divorced|paleshkov8u@typepad.com|202-645-3831|0245 Village Center,Washington,District of Columbia|unknown|11/21/2013|
|AMANDI JENSEN|27|divorced|ajensena3@netscape.com|434-883-4657|04024 Miller Lane,Charlottesville,Virginia|unknown|5/2/2019|
|ATLANTE LEES|31|divorced|aleesbl@hc360.com|801-707-4073|007 John Wall Crossing,Provo,Utah|unknown|2/18/2021|
|MICHAELINE SHARPLE|37|married|msharpled5@myspace.com|864-363-4730|82865 Vahlen Plaza,Anderson,South Carolina|unknown|6/15/2020|
|MADELINE MCALESS|43|divorced|mmcaless2@tuttocitta.it|941-348-5721|7211 Mariners Cove Plaza,Naples,Florida|unknown|11/12/2015|
|JANITH WHALES|55|married|jwhales7@woothemes.com|937-141-1241|54438 Anhalt Drive,Hamilton,Ohio|unknown|1/21/2022|
|ELAYNE JODKOWSKI|65|divorced|ejodkowskif@bloglines.com|404-120-0505|7750 Blue Bill Park Terrace,Atlanta,Georgia|unknown|11/9/2017|
|DUKE TIE|41|divorced|dtie1o@opensource.org|217-106-2204|29 Bunker Hill Plaza,Champaign,Illinois|unknown|9/18/2021|
|EULA IXOR|40|married|eixor27@eepurl.com|202-633-7657|8 Sunnyside Point,Washington,District of Columbia|unknown|5/5/2019|
|JAMMIE MCMURRAYA|68|married|jmcmurraya2j@cbslocal.com|503-871-2610|5 Butternut Crossing,Salem,Oregon|unknown|5/8/2017|
|SHANDA SYBBE|68|married|ssybbe4h@studiopress.com|202-837-6841|759 Rusk Alley,Washington,Districts of Columbia|unknown|11/6/2017|
|ROMAIN WISE|44|married|rwise4o@prlog.org|323-621-5722|63098 Lukken Junction,Los Angeles,California|unknown|5/8/2020|
|ROXY TWEEDELL|28|single|rtweedell64@mayoclinic.com|702-774-4320|5490 Burrows Lane,Las Vegas,Nevada|unknown|5/27/2017|
|BORIS VELLDEN|55|divorced|bvellden85@cornell.edu|941-500-2543|6 Farragut Circle,Sarasota,Florida|unknown|3/14/2019|
|ETTIE VON DER EMPTEN|46|married|evon92@state.gov|615-375-3691|5080 Forest Pass,Nashville,Tennessee|unknown|2/1/2022|
|DONNY PADDEFIELD|42|single|dpaddefieldac@blogspot.com|602-776-1099|424 Westend Circle,Phoenix,Arizona|unknown|4/8/2017|
|GRADEY PAULITSCHKE|29|divorced|gpaulitschkeaq@ustream.tv|860-472-7819|37412 Banding Circle,Hartford,Connecticut|unknown|8/7/2016|
|WENDELL MCCRITCHIE|50|married|wmccritchieax@jalbum.net|716-258-7121|1 Dottie Hill,Buffalo,New York|unknown|8/8/2021|
|LEFTY GYORGY|18|divorced|lgyorgycd@edublogs.org|603-268-7110|151 Brown Plaza,Portsmouth,New Hampshire|unknown|1/30/2021|
|RODD HAVERSON|59|single|rhaversond1@mtv.com|562-775-9809|8428 Lakewood Gardens Street,Long Beach,California|unknown|11/25/2017|
|SUNSHINE DUNBLETON|63|unknown|sdunbletonee@yellowpages.com|704-231-0109|79497 Milwaukee Point,Charlotte,North Carolina|unknown|2/10/2018|
|PASQUALE MEEKINS|56|single|pmeekinsg3@sakura.ne.jp|816-575-3947|54830 Northport Crossing,Kansas City,Missouri|unknown|1/29/2017|
|PRYCE VANE|57|married|pvaneha@economist.com|857-566-9825|47983 7th Point,Newton,Massachusetts|unknown|8/17/2015|
|SHANDEIGH ORMAN|50|married|sormanhs@mlb.com|410-721-9221|58985 Waxwing Parkway,Baltimore,Maryland|unknown|12/25/2015|
|NIKI LORENZETTO|62|single|nlorenzettoj8@ustream.tv|310-811-8279|7 Ronald Regan Parkway,Inglewood,California|unknown|1/20/2021|
|MERWYN SANDBROOK|44|married|msandbrookjm@patch.com|585-244-8825|8 Buhler Center,Rochester,New York|unknown|4/29/2015|
|GABRILA VILLIERS|67|married|gvilliersl7@bandcamp.com|609-567-3769|16575 Northland Point,Trenton,New Jersey|unknown|5/22/1921|
|SHINA DIETZLER|54|married|sdietzlerld@apache.org|336-591-9564|6 Gale Place,Greensboro,North Carolina|unknown|1/28/2019|
|CARLINA WINWARD|65|married|cwinwardmx@mit.edu|682-190-0341|4826 Ruskin Hill,Fort Worth,Texas|unknown|11/23/2019|
|CHEV SWINDELLS|44|married|cswindellsn9@examiner.com|505-154-1912|20365 Mockingbird Hill,Albuquerque,New Mexico|unknown|3/29/2015|
|GIOVANNA FAITHFULL|61|divorced|gfaithfullnm@mozilla.org|713-284-0232|45171 Corry Alley,Houston,Texas|unknown|10/8/2019|
|SHAYLYN JANKOVIC|37|single|sjankovicnp@booking.com|412-301-5395|08 Continental Hill,Pittsburgh,Pennsylvania|unknown|6/23/2016|
|GODDART STEYNOR|45|married|gsteynorqd@mtv.com|610-550-5052|939 Evergreen Park,Philadelphia,Pennsylvania|unknown|9/8/2017|
|SHAE DITCHETT|25|married|sditchettqj@reference.com|203-505-3211|05799 Merchant Trail,Bridgeport,Connecticut|unknown|11/16/2021|
|HURLEY ABTHORPE|53|married|habthorpeqp@creativecommons.org|203-547-5101|6572 Macpherson Court,Hartford,Connecticut|unknown|11/28/2017|

## Step 7: Customize and standardize the 'membership_' column data
Hypothesis: membership dates only start from 2000 onwards
### Filter irrelevant data
```SQL
SELECT membership_date,
       SUBSTR(membership_date, LENGTH(membership_date) - 3, 4) AS year
FROM club_member_info_cleaned
WHERE SUBSTR(membership_date, LENGTH(membership_date) - 3, 4) < '2000';
```
|membership_date|year|
|---------------|----|
|3/12/1921|1921|
|10/1/1912|1912|
|2/20/1916|1916|
|5/8/1912|1912|
|10/4/1919|1919|
|3/10/1913|1913|
|1/8/1912|1912|
|9/2/1914|1914|
|5/11/1916|1916|
|1/31/1915|1915|
|5/15/1915|1915|
|3/3/1917|1917|
|4/30/1915|1915|
|5/22/1921|1921|
|10/27/1915|1915|
|7/5/1915|1915|
### Update invalid data to unknown
```SQL
UPDATE club_member_info_cleaned
SET membership_date = 'unknown'
WHERE SUBSTR(membership_date, LENGTH(membership_date) - 3, 4) < '2000';
```
The result:
```SQL
SELECT * FROM club_member_info_cleaned
WHERE membership_date = 'unknown';
```
|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|
|MENDIE ALEXANDRESCU|46|single|malexandrescu8@state.gov|504-918-4753|34 Delladonna Terrace,New Orleans,Louisiana|Systems Administrator III|unknown|
|ANNALIESE ETOILE|52|married|aetoile2w@artisteer.com|unknown|43 Nova Circle,Garden Grove,California|Financial Advisor|unknown|
|SIBELLE KORT|51|divorced|skort5n@wufoo.com|858-990-5533|906 Rockefeller Pass,San Diego,California|Accountant III|unknown|
|TOWN LAMBE|38|married|tlambeak@rediff.com|202-931-4461|3 Crest Line Plaza,Washington,District of Columbia|Environmental Tech|unknown|
|ULRIKE TIMMENS|36|divorced|utimmensd3@nyu.edu|201-321-2529|86 Northfield Crossing,Jersey City,New Jersey|Clinical Specialist|unknown|
|KARALYNN JELFS|33|married|kjelfshq@wordpress.com|616-732-7007|30 Clove Park,Grand Rapids,Michigan|Cost Accountant|unknown|
|CARLOS MACHIN|41|divorced|cmachinj0@xinhuanet.com|215-833-2589|91 Anzinger Alley,Philadelphia,Pennsylvania|Programmer I|unknown|
|GABY HASKINS|37|single|ghaskinsl7@canalblog.com|702-717-5486|293 Pleasure Plaza,Las Vegas,Nevada|Senior Financial Analyst|unknown|
|HOBEY GWYNNE|46|single|hgwynneod@cafepress.com|901-568-9545|30 Rockefeller Lane,Memphis,Tennessee|Senior Quality Engineer|unknown|
|BLINNY TATTERSILL|27|divorced|btattersill7r@tmall.com|503-762-5427|6699 Di Loreto Avenue,Portland,Oregon|Marketing Assistant|unknown|
|ELBERTINE GUILBERT|52|married|eguilberteb@ted.com|619-613-6303|1965 Texas Point,San Diego,California|Financial Analyst|unknown|
|MERRIDIE O' DORNAN|51|married|mofo@theglobeandmail.com|865-938-1990|53585 Pepper Wood Way,Knoxville,Tennessee|Budget/Accounting Analyst II|unknown|
|THOMAS LONDSDALE|19|single|tlondsdalek8@ifeng.com|716-702-8514|2 Cherokee Circle,Buffalo,New York|Help Desk Operator|unknown|
|GABRILA VILLIERS|67|married|gvilliersl7@bandcamp.com|609-567-3769|16575 Northland Point,Trenton,New Jersey|unknown|unknown|
|GAYEL JAFFREY|61|single|gjaffreypb@hatena.ne.jp|213-603-4464|68 Anhalt Street,Los Angeles,California|Human Resources Assistant II|unknown|
|BRITNEY HERRIEVEN|38|married|bherrievenqv@ask.com|713-204-6332|68027 Clyde Gallagher Hill,Houston,Texas|Dental Hygienist|unknown|

## Data after cleaning
```SQL
SELECT * FROM club_member_info_cleaned 
LIMIT 10;
```
The result:
|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|
|ADDIE LUSH|40|married|alush0@shutterfly.com|254-389-8708|3226 Eastlawn Pass,Temple,Texas|Assistant Professor|7/31/2013|
|ROCK CRADICK|46|married|rcradick1@newsvine.com|910-566-2007|4 Harbort Avenue,Fayetteville,North Carolina|Programmer III|5/27/2018|
|SYDEL SHARVELL|46|divorced|ssharvell2@amazon.co.jp|702-187-8715|4 School Place,Las Vegas,Nevada|Budget/Accounting Analyst I|10/6/2017|
|CONSTANTIN DE LA CRUZ|35|unknown|co3@bloglines.com|402-688-7162|6 Monument Crossing,Omaha,Nebraska|Desktop Support Technician|10/20/2015|
|GAYLOR REDHOLE|38|married|gredhole4@japanpost.jp|917-394-6001|88 Cherokee Pass,New York City,New York|Legal Assistant|5/29/2019|
|WANDA DEL MAR|44|single|wkunzel5@slideshare.net|937-467-6942|10864 Buhler Plaza,Hamilton,Ohio|Human Resources Assistant IV|3/24/2015|
|JOANN KENEALY|41|married|jkenealy6@bloomberg.com|513-726-9885|733 Hagan Parkway,Cincinnati,Ohio|Accountant IV|4/17/2013|
|JOETE CUDIFF|51|divorced|jcudiff7@ycombinator.com|616-617-0965|975 Dwight Plaza,Grand Rapids,Michigan|Research Nurse|11/16/2014|
|MENDIE ALEXANDRESCU|46|single|malexandrescu8@state.gov|504-918-4753|34 Delladonna Terrace,New Orleans,Louisiana|Systems Administrator III|unknown|
|FEY KLOSS|52|married|fkloss9@godaddy.com|808-177-0318|8976 Jackson Park,Honolulu,Hawaii|Chemical Engineer|11/5/2014|

