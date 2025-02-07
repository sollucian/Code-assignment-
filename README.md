import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Simulating synthetic data for network access logs
np.random.seed(42)
num_records = 1000

data = {
    "employee_id": np.random.randint(1000, 1100, num_records),
    "access_time": pd.date_range(start="2024-01-01", periods=num_records, freq="H"),
    "data_transferred_MB": np.random.exponential(scale=10, size=num_records),
    "failed_logins": np.random.binomial(n=5, p=0.1, size=num_records),
    "privilege_escalation": np.random.choice([0, 1], size=num_records, p=[0.95, 0.05])
}

df = pd.DataFrame(data)

# Detecting anomalies: High data transfer or frequent failed logins
df["anomaly"] = (df["data_transferred_MB"] > df["data_transferred_MB"].quantile(0.95)) | (df["failed_logins"] > 3)

# Plot visualization
plt.figure(figsize=(12, 6))
sns.scatterplot(data=df, x="access_time", y="data_transferred_MB", hue="anomaly", palette={False: "blue", True: "red"})
plt.title("Network Data Transfer Anomalies Over Time")
plt.xlabel("Access Time")
plt.ylabel("Data Transferred (MB)")
plt.xticks(rotation=45)
plt.legend(title="Anomaly", labels=["Normal", "Suspicious"])
plt.show()

