# purrr_many_csvfiles_reading

# 필요한 패키지를 설치합니다. 
# install.packages(c("tidyverse", "fs"))

library(tidyverse)  # purrr와 readr을 불러옵니다.
library(fs)

data_dir <- "D:/Non_Documents/AI/R/data/ie-general-referrals-by-hospital"

fs::dir_ls(data_dir)


csv_files <- fs::dir_ls(data_dir, regexp = "\\.csv$")
csv_files


readr::read_csv(csv_files[1])


csv_files %>% 
  map_dfr(read_csv)


csv_files %>% 
  map_dfr(read_csv, col_types = cols("Month_Year" = col_date(format = "%b-%y")))


library(lubridate)

csv_files %>% 
  map_dfr(read_csv) %>%
  mutate(Month_Year = myd(Month_Year, truncated = 1))


csv_files %>% 
  map_dfr(read_csv, .id = "source") %>%
  mutate(Month_Year = myd(Month_Year, truncated = 1))




# 최종코드 --------------------------------------------------------------------


library(tidyverse)  # purrr와 readr을 불러옵니다.
library(fs)
library(lubridate)

data_dir <- "D:/Non_Documents/AI/R/data/ie-general-referrals-by-hospital"

df <- csv_files %>% 
  map_dfr(read_csv, .id = "source") %>%
  mutate(Month_Year = myd(Month_Year, truncated = 1))


head(df)
