# Web_scraping_R
Using R language Studio performed web scraping of top 10 data/business analytics companies in India.
# R script

#data collection for to data/business analytics companies in india

library(tidyverse)

 html_dump<- read_html("https://www.analytixlabs.co.in/blog/top-data-analytics-companies-in-india/")

# reading names from htlm nodes
CompanyName <- html_dump %>% 
  html_nodes(".entry-content h3") %>% 
  html_text()

CompanyName # viewing if we got correct names

x <- str_split_fixed(CompanyName," ",2) # gives a character matrix
CompanyName <- x[,2] #inputing second column of character matrix as company names

#getting No. of employees in each company
Total_employees <- html_dump %>% 
  html_nodes("p+ ul li:nth-child(1)") %>% 
  html_text() 
#data cleaning needed
Temp <- Total_employees %>% 
  str_starts("Total") %>%  #select line start with "Total
  Total_employees[.] %>%    
  str_split_fixed(": ",2)  #splitting the string at ": "
  
Total_employees <-  append(NA,c(Temp[,2]))# adding one NA in front of the 1st company
  
#headquarter location outside India
Headquarters  <- html_dump %>% 
  html_nodes("#main li:nth-child(2)") %>% 
  html_text() 

Temp1 <- Headquarters %>% 
  str_starts("Headquarter") %>%
  x[.] %>% 
  str_split_fixed(": ",2)

Headquarters <- append(NA,c(Temp1[,2]))

#location in India
Location_India <- html_dump %>% 
  html_nodes("#main li:nth-child(3)") %>% 
  html_text()

Temp2 <- Location_India %>% 
  str_starts("Location") %>% 
  Location_India[.] %>% 
  str_split_fixed(": ",2)

Location_India <- append(NA,c(Temp2[,2]))    

# glassdoor rating
Glassdoor_rating <- html_dump %>% 
  html_nodes("ul+ p") %>% 
  html_text()

Temp3 <- Glassdoor_rating %>% 
  str_starts("Glass") %>% 
  Glassdoor_rating[.] %>% 
  str_split_fixed("C",2)

Temp31 <- Temp3[,1] %>% # two slipts
  str_split_fixed(": ",2)

Glassdoor_rating <- append(NA,c(Temp31[,2]))

#creating a data frame
Analytics_Companies <- data.frame(CompanyName,Total_employees,Headquarters,Location_India,Glassdoor_rating)
  
# saving the data frame in a csv file
write.csv(Analytics_Companies,"Analytics_Companies_data.csv")
