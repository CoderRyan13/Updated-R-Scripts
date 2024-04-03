# importing the library
library("devtools")
devtools::install_github("arthur-shaw/susoapi")
library("susoapi")
library("tidyverse")
library("haven")
library("plyr")

library(DBI)
library(RMySQL)
library(janitor)

# creating a database connection
connection <- dbConnect(RMySQL::MySQL(), 
                        dbname = "survey_dashboards", 
                        host = "200.32.244.19", 
                        port = 3306, 
                        user = "gaguilar", 
                        password = "P@ssw0rd10")


set_credentials(
  server = "http://hqsurveys.sib.org.bz",
  workspace = "hbs",
  user = "gapi",
  password = "API4statistics!"
)

# get a dataframe of questionnaires and their attributes
# all_questionnaires <- get_questionnaires()

# STEP1: START AN EXPORT JOB
# specifying same same options as in user interface
# optionally specifying other options--including some not available in the UI
start_export(
  qnr_id = "8f467cac973940b184efec0944aea449$1",
  export_type = "SPSS",
  interview_status = "All",
  include_meta = TRUE
) -> started_job_id

# STEP 2: CHECK EXPORT JOB PROGESS, UNTIL COMPLETE
# specifying ID of job started in prior step

while (TRUE){
  res <- get_export_job_details(job_id = started_job_id)
  
  status <- res$ExportStatus 
  
  if (status == 'Completed'){
    break
  }
}


# STEP 3: DOWNLOAD THE EXPORT FILE, ONCE THE JOB IS COMPLETE
# specifying:
# - job ID
# - where to download the file
get_export_file(
  job_id = started_job_id,
 # path = "C:\\Users\\sib_temp\\Documents\\R Script\\Script"
 path = './'
  # path = "/home/rshiny/airflow/scripts"
)

# extracting into given folder

unzip('belize_hbs_4_1_SPSS_All.zip',
      exdir = './SPSS_FOLDER/')

# Load extracted files into R

# names(belize_hbs_4)

# belize_hbs_4 |>
  # janitor::tabyl(ER9)

belize_hbs_4 <- read_sav('./SPSS_FOLDER/belize_hbs_4.sav', user_na = TRUE)
consumption_pattern_roster <- read_sav('./SPSS_FOLDER/consumption_pattern_roster.sav', user_na = TRUE)
FF1_Roster <- read_sav('./SPSS_FOLDER/FF1Roster.sav', user_na = TRUE) 
FF2_Roster <- read_sav('./SPSS_FOLDER/FF2Roster.sav', user_na = TRUE) 
FF3_Roster <- read_sav('./SPSS_FOLDER/FF3Roster.sav', user_na = TRUE) 
FF4_Roster <- read_sav('./SPSS_FOLDER/FF4Roster.sav', user_na = TRUE)
FF5_Roster <- read_sav('./SPSS_FOLDER/FF5Roster.sav', user_na = TRUE)
FF6_Roster <- read_sav('./SPSS_FOLDER/FF6Roster.sav', user_na = TRUE)
FF7_Roster <- read_sav('./SPSS_FOLDER/FF7Roster.sav', user_na = TRUE)
FF8_Roster <- read_sav('./SPSS_FOLDER/FF8Roster.sav', user_na = TRUE)
FF9_Roster <- read_sav('./SPSS_FOLDER/FF9Roster.sav', user_na = TRUE)
FF10_Roster <- read_sav('./SPSS_FOLDER/FF10Roster.sav', user_na = TRUE)
FF11_Roster <- read_sav('./SPSS_FOLDER/FF11Roster.sav', user_na = TRUE)
FF12_Roster <- read_sav('./SPSS_FOLDER/FF12Roster.sav', user_na = TRUE)
FF13_Roster <- read_sav('./SPSS_FOLDER/FF13Roster.sav', user_na = TRUE)
FF14_Roster <- read_sav('./SPSS_FOLDER/FF14Roster.sav', user_na = TRUE)
FF15_Roster <- read_sav('./SPSS_FOLDER/FF15Roster.sav', user_na = TRUE)
FF16_Roster <- read_sav('./SPSS_FOLDER/FF16Roster.sav', user_na = TRUE)
FF17_Roster <- read_sav('./SPSS_FOLDER/FF17Roster.sav', user_na = TRUE)
FF18_Roster <- read_sav('./SPSS_FOLDER/FF18Roster.sav', user_na = TRUE)
FF19_Roster <- read_sav('./SPSS_FOLDER/FF19Roster.sav', user_na = TRUE)
FF20_Roster <- read_sav('./SPSS_FOLDER/FF20Roster.sav', user_na = TRUE)
listing_roster <- read_sav('./SPSS_FOLDER/listingroster.sav', user_na = TRUE)
TR2_1 <- read_sav('./SPSS_FOLDER/TR2_1.sav', user_na = TRUE)
TR2_2 <- read_sav('./SPSS_FOLDER/TR2_2.sav', user_na = TRUE)

# Test if connection was made
# query <- "show tables";
# result <- dbGetQuery(connection,query);
# print(result)

# Create tables
# dbWriteTable(connection,"hbs_household",belize_hbs_4,overwrite=T)
# dbReadTable(connection, "hbs_household")

dbWriteTable(connection,"hbs_listing_roster",listing_roster,overwrite=T)
dbReadTable(connection, "hbs_listing_roster")

dbWriteTable(connection,"hbs_consumption_pattern_roster",consumption_pattern_roster,overwrite=T)
dbReadTable(connection, "hbs_consumption_pattern_roster")

dbWriteTable(connection,"hbs_transportation_1",TR2_1,overwrite=T)
dbReadTable(connection, "hbs_transportation_1")

dbWriteTable(connection,"hbs_transportation_2",TR2_2,overwrite=T)
dbReadTable(connection, "hbs_transportation_2")

# Joins FF_Roster together

# renaming columns and changing their type to factor
# renaming so that we can rbind them
# changing to factor so we can put them all into a single column

f1 <- FF1_Roster |>
  dplyr::rename(
    id = FF1Roster__id,
    B = FF1B,
    C = FF1C,
    D = FF1D,
    E = FF1E,
    F = FF1F
  ) |>
  mutate(
    id = paste0('FF1_', id),
    B = as_factor(B),
    C = as_factor(C),
    D = as_factor(D),
    E = as_factor(E),
    F = as_factor(F)
  )

f2 <- FF2_Roster |>
  dplyr::rename(
    id = FF2Roster__id,
    B = FF2B,
    C = FF2C,
    D = FF2D,
    E = FF2E,
    F = FF2F
  ) |>
  mutate(
    id = paste0('FF2_', id),
    B = as_factor(B),
    C = as_factor(C),
    D = as_factor(D),
    E = as_factor(E),
    F = as_factor(F)
  )

f3 <- FF3_Roster |>
  dplyr::rename(
    id = FF3Roster__id,
    B = FF3B,
    C = FF3C,
    D = FF3D,
    E = FF3E,
    F = FF3F
  ) |>
  mutate(
    id = paste0('FF3_', id),
    B = as_factor(B),
    C = as_factor(C),
    D = as_factor(D),
    E = as_factor(E),
    F = as_factor(F)
  )

f4 <- FF4_Roster |>
  dplyr::rename(
    id = FF4Roster__id,
    B = FF4B,
    C = FF4C,
    D = FF4D,
    E = FF4E,
    F = FF4F
  ) |>
  mutate(
    id = paste0('FF4_', id),
    B = as_factor(B),
    C = as_factor(C),
    D = as_factor(D),
    E = as_factor(E),
    F = as_factor(F)
  )

f5 <- FF5_Roster |>
  dplyr::rename(
    id = FF5Roster__id,
    B = FF5B,
    C = FF5C,
    D = FF5D,
    E = FF5E,
    F = FF5F
  ) |>
  mutate(
    id = paste0('FF5_', id),
    B = as_factor(B),
    C = as_factor(C),
    D = as_factor(D),
    E = as_factor(E),
    F = as_factor(F)
  )

f6 <- FF6_Roster |>
  dplyr::rename(
    id = FF6Roster__id,
    B = FF6B,
    C = FF6C,
    D = FF6D,
    E = FF6E,
    F = FF6F
  ) |>
  mutate(
    id = paste0('FF6_', id),
    B = as_factor(B),
    C = as_factor(C),
    D = as_factor(D),
    E = as_factor(E),
    F = as_factor(F)
  )

f7 <- FF7_Roster |>
  dplyr::rename(
    id = FF7Roster__id,
    B = FF7B,
    C = FF7C,
    D = FF7D,
    E = FF7E,
    F = FF7F
  ) |>
  mutate(
    id = paste0('FF7_', id),
    B = as_factor(B),
    C = as_factor(C),
    D = as_factor(D),
    E = as_factor(E),
    F = as_factor(F)
  )

f8 <- FF8_Roster |>
  dplyr::rename(
    id = FF8Roster__id,
    B = FF8B,
    C = FF8C,
    D = FF8D,
    E = FF8E,
    F = FF8F
  ) |>
  mutate(
    id = paste0('FF8_', id),
    B = as_factor(B),
    C = as_factor(C),
    D = as_factor(D),
    E = as_factor(E),
    F = as_factor(F)
  )

f9 <- FF9_Roster |>
  dplyr::rename(
    id = FF9Roster__id,
    B = FF9B,
    C = FF9C,
    D = FF9D,
    E = FF9E,
    F = FF9F
  ) |>
  mutate(
    id = paste0('FF9_', id),
    B = as_factor(B),
    C = as_factor(C),
    D = as_factor(D),
    E = as_factor(E),
    F = as_factor(F)
  )

f10 <- FF10_Roster |>
  dplyr::rename(
    id = FF10Roster__id,
    B = FF10B,
    C = FF10C,
    D = FF10D,
    E = FF10E,
    F = FF10F
  ) |>
  mutate(
    id = paste0('FF10_', id),
    B = as_factor(B),
    C = as_factor(C),
    D = as_factor(D),
    E = as_factor(E),
    F = as_factor(F)
  )

f11 <- FF11_Roster |>
  dplyr::rename(
    id = FF11Roster__id,
    B = FF11B,
    C = FF11C,
    D = FF11D,
    E = FF11E,
    F = FF11F
  ) |>
  mutate(
    id = paste0('FF11_', id),
    B = as_factor(B),
    C = as_factor(C),
    D = as_factor(D),
    E = as_factor(E),
    F = as_factor(F)
  )

f12 <- FF12_Roster |>
  dplyr::rename(
    id = FF12Roster__id,
    B = FF12B,
    C = FF12C,
    D = FF12D,
    E = FF12E,
    F = FF12F
  ) |>
  mutate(
    id = paste0('FF12_', id),
    B = as_factor(B),
    C = as_factor(C),
    D = as_factor(D),
    E = as_factor(E),
    F = as_factor(F)
  )

f13 <- FF13_Roster |>
  dplyr::rename(
    id = FF13Roster__id,
    B = FF13B,
    C = FF13C,
    D = FF13D,
    E = FF13E,
    F = FF13F
  ) |>
  mutate(
    id = paste0('FF13_', id),
    B = as_factor(B),
    C = as_factor(C),
    D = as_factor(D),
    E = as_factor(E),
    F = as_factor(F)
  )

f14 <- FF14_Roster |>
  dplyr::rename(
    id = FF14Roster__id,
    B = FF14B,
    C = FF14C,
    D = FF14D,
    E = FF14E,
    F = FF14F
  ) |>
  mutate(
    id = paste0('FF14_', id),
    B = as_factor(B),
    C = as_factor(C),
    D = as_factor(D),
    E = as_factor(E),
    F = as_factor(F)
  )

f15 <- FF15_Roster |>
  dplyr::rename(
    id = FF15Roster__id,
    B = FF15B,
    C = FF15C,
    D = FF15D,
    E = FF15E,
    F = FF15F
  ) |>
  mutate(
    id = paste0('FF15_', id),
    B = as_factor(B),
    C = as_factor(C),
    D = as_factor(D),
    E = as_factor(E),
    F = as_factor(F)
  )

f16 <- FF16_Roster |>
  dplyr::rename(
    id = FF16Roster__id,
    B = FF16B,
    C = FF16C,
    D = FF16D,
    E = FF16E,
    F = FF16F
  ) |>
  mutate(
    id = paste0('FF16_', id),
    B = as_factor(B),
    C = as_factor(C),
    D = as_factor(D),
    E = as_factor(E),
    F = as_factor(F)
  )

f17 <- FF17_Roster |>
  dplyr::rename(
    id = FF17Roster__id,
    B = FF17B,
    C = FF17C,
    D = FF17D,
    E = FF17E,
    F = FF17F
  ) |>
  mutate(
    id = paste0('FF17_', id),
    B = as_factor(B),
    C = as_factor(C),
    D = as_factor(D),
    E = as_factor(E),
    F = as_factor(F)
  )

f18 <- FF18_Roster |>
  dplyr::rename(
    id = FF18Roster__id,
    B = FF18B,
    C = FF18C,
    D = FF18D,
    E = FF18E,
    F = FF18F
  ) |>
  mutate(
    id = paste0('FF18_', id),
    B = as_factor(B),
    C = as_factor(C),
    D = as_factor(D),
    E = as_factor(E),
    F = as_factor(F)
  )

f19 <- FF19_Roster |>
  dplyr::rename(
    id = FF19Roster__id,
    B = FF19B,
    C = FF19C,
    D = FF19D,
    E = FF19E,
    F = FF19F
  ) |>
  mutate(
    id = paste0('FF19_', id),
    B = as_factor(B),
    C = as_factor(C),
    D = as_factor(D),
    E = as_factor(E),
    F = as_factor(F)
  )

f20 <- FF20_Roster |>
  dplyr::rename(
    id = FF20Roster__id,
    B = FF20B
  ) |>
  mutate(
    id = paste0('FF20_', id),
    B = as_factor(B)
  )

# combine them into one data.frame
f_temp <- rbind(f1, f2, f3, f4, f5, f6, f7, f8, f9, f10, f11, f12, f13,
                f14, f15, f16, f17, f18, f19, f20) 

# convert to long format and add unique ID for each item/roster/question

f_temp <- f_temp |>
  pivot_longer(
    cols = 4:8,
    names_to = 'Question',
    values_to = 'Answer'
  ) |>
  mutate(
    id_full = paste0(id, Question)
  ) |>
  relocate(
    id_full,
    .after = 'interview__id'
  ) |> select(-c(id, Question))
  
# widen into final format

f_final <- f_temp |>  
  pivot_wider(names_from = id_full, values_from = Answer)

# FF1_Roster |>
  # full_join(FF2_Roster, by = c('interview__key', 'interview__id')) |>
  # full_join(FF3_Roster,  by = c('interview__key', 'interview__id'), relationship = 'many-to-many') |> View()



dbWriteTable(connection,"hbs_ff_roster",f_final,overwrite=T)
dbReadTable(connection, "hbs_ff_roster")

# Disconnect from database
dbDisconnect(connection)