# dsima
just testing# Install & load packages
packages <- c("ggplot2", "dplyr")
lapply(packages, function(p) if (!require(p, character.only = TRUE)) install.packages(p))

library(ggplot2)
library(dplyr)

# ---- Simulate patient vital signs ----
set.seed(77)
patients <- data.frame(
  PatientID = 1:100,
  HeartRate = rnorm(100, mean = 75, sd = 12),
  OxygenSat = rnorm(100, mean = 96, sd = 2)
)

# ---- Run K-means clustering (2 groups: Normal vs Abnormal) ----
set.seed(123)
km <- kmeans(patients[, c("HeartRate", "OxygenSat")], centers = 2)
patients$Cluster <- factor(km$cluster, labels = c("Group 1", "Group 2"))

# ---- Plot clusters ----
ggplot(patients, aes(x = HeartRate, y = OxygenSat, color = Cluster)) +
  geom_point(size = 3, alpha = 0.8) +
  geom_point(data = as.data.frame(km$centers), 
             aes(x = HeartRate, y = OxygenSat),
             color = "black", shape = 8, size = 4) +
  labs(title = "Clustering of Patients by Heart Rate & Oxygen Saturation",
       x = "Heart Rate (bpm)",
       y = "Oxygen Saturation (%)") +
  theme_minimal()

