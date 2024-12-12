# Stack Overflow Programming Language Trends Analysis with R

## Description
In this project, we will analyze the changing popularity trends of programming languages over time using data from Stack Overflow. Stack Overflow, a leading Q&A site for programmers, provides a rich dataset with millions of programming-related questions. Each question is tagged with specific topics or technologies, such as R, Python, Java, and JavaScript. By analyzing the number of questions related to each tag, we can estimate the relative popularity of different programming languages over the years.

The analysis aims to:
- Investigate how the number of questions tagged with "R" compares to the total number of questions each year.
- Calculate the percentage of all questions in 2020 tagged with "R".
- Identify the five programming language tags with the highest total number of questions asked between 2015 and 2020.
- Identify which tag experienced the largest year-over-year increase in its percentage of questions.

The insights gathered from this analysis will help you understand the shifting trends in the programming community, highlighting languages that are gaining or losing popularity. This can be beneficial for developers, data scientists, and others interested in the evolving landscape of programming languages.

## Steps Involved

### 1. Identify the R Trends
We calculate the percentage of questions tagged with "R" each year and compare it with the total number of questions asked across all tags. This data will be saved in a dataframe named `r_over_time`, which will contain the following columns: `year`, `tag`, `num_questions`, `year_total`, and `percentage`.

```r
r_over_time <- data %>%
  filter(tag == "r") %>%
  mutate(percentage = num_questions / year_total * 100) %>%
  select(year, tag, num_questions, year_total, percentage)

# View the r_over_time data
head(r_over_time)
````

### 2. Find the Percentage of R Questions in 2020
For the year 2020, we calculate the percentage of questions tagged with "R" as a fraction of the total number of questions in that year.
```r
# Use the year_total from 2020 directly
total_questions_2020 <- data %>%
  filter(year == 2020) %>%
  summarise(year_total = max(year_total)) %>%
  pull(year_total)

# Then, get the number of questions tagged with "R" in 2020
r_questions_2020 <- data %>%
  filter(tag == "r", year == 2020) %>%
  summarise(num_questions = sum(num_questions)) %>%
  pull(num_questions)

# Calculate the percentage for R in 2020
r_percentage <- (r_questions_2020 / total_questions_2020) * 100
print(paste("Percentage of R-tagged questions in 2020:", r_percentage))
````

### 3. Calculate the Five Most Asked-About Tags Between 2016-2020
To identify the five most popular programming languages from 2015 to 2020, we will filter the dataset for this period and calculate the total number of questions tagged with each language. We will then sort the tags by total question count and extract the top five.
```r
highest_tags <- data %>%
  filter(year >= 2015, year <= 2020) %>%
  group_by(tag) %>%
  summarise(total_questions = sum(num_questions)) %>%
  arrange(desc(total_questions)) %>%
  slice_max(order_by = total_questions, n = 5) %>%
  pull(tag)

# View the highest tags
highest_tags
````

### 4. Find the Tag with the Largest Year-over-Year Increase in Its Percentage of Questions
In this step, we will calculate the percentage of questions for each tag, per year, and identify the tag that has shown the largest year-over-year increase in its percentage. This will provide insights into which languages are gaining popularity the fastest.

```r
tag_yearly_change <- data %>%
  group_by(tag) %>%
  arrange(year) %>%
  mutate(percentage = num_questions / year_total * 100,
         percentage_ratio = percentage / lag(percentage)) %>%
  filter(!is.na(percentage_ratio)) %>%
  ungroup()

# Find the tag with the largest year-over-year increase (highest ratio)
highest_ratio_tag <- tag_yearly_change %>%
  slice_max(order_by = percentage_ratio, n = 1) %>%
  pull(tag)

# View the tag with the largest year-over-year increase
highest_ratio_tag
````

### Bonus: Data Visualization
We will also visualize the trends of "R"-tagged questions over time to help us better understand the patterns and changes in popularity. Using ggplot2, we can plot the percentage of R-tagged questions for each year.

```r
ggplot(r_over_time, aes(x = year, y = percentage)) +
  geom_line(color = "blue") +
  geom_point() +
  labs(title = "Percentage of Stack Overflow Questions Tagged with 'R' Over Time",
       x = "Year", y = "Percentage")
````
### Conclusion
By following these steps, you will uncover important insights into the changing trends of programming languages over time. You'll be able to identify which programming languages, such as R, Python, Java, or JavaScript, are gaining or losing popularity based on the number of questions asked on Stack Overflow. This project demonstrates how to analyze trends in the programming community and can help developers and data scientists focus their learning efforts on the most relevant technologies.

### Insights

- **Trends in the Popularity of "R" Over Time:**
The data reveals that the percentage of questions tagged with "R" has fluctuated over the years. By tracking these fluctuations, we can understand how the demand for knowledge about R has evolved. A significant rise or drop in the number of questions tagged with "R" could be a direct indicator of the growing or declining interest in this language among developers and data scientists.

- **Comparison with Other Programming Languages:**
The five most frequently asked-about programming languages between 2015 and 2020 provide a snapshot of the trends in the programming community. If "R" is consistently in the top five, it indicates that it remains a popular choice among users of Stack Overflow.

- **Year-over-Year Changes:**
By identifying the tag that experienced the largest year-over-year increase in its percentage of questions, we can pinpoint languages that are gaining momentum. This is an essential factor for understanding emerging trends and which technologies might dominate in the future.

- **R's Percentage in 2020:**
The percentage of questions tagged with "R" in 2020 helps evaluate the relative importance of R within the context of other programming languages. If this percentage is low compared to languages like Python or JavaScript, it may indicate areas where the language has lost popularity or where competition is intensifying.

---

### Disclaimer

This project uses data sourced from [Stack Exchange Data Explorer](https://data.stackexchange.com/) for educational purposes. All intellectual property rights related to the dataset belong to Stack Overflow. This analysis is strictly for non-commercial use and is intended for personal and educational purposes only.

The dataset and analysis are based on the educational materials provided by [DataCamp](https://www.datacamp.com/). All rights related to the content, methodology, and materials belong to DataCamp. This project is shared for educational purposes and personal portfolio development only.


