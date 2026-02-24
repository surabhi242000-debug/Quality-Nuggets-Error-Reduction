# Project: Quality-Nuggets-Error-Reduction

## 📌 Project Overview

The "Quality Nuggets" initiative was an end-to-end data-driven strategy designed to improve outbound quality by 10%. By merging operational outbound logs with error audit data, I identified systemic "Grey Zones" where agent confusion led to incorrect case resolutions.

## 🛠️ The SQL Data Engine

To build the master dataset, I performed a RIGHT JOIN between the Outbound Dataset and the Error Dataset. This ensured we captured all audited errors while retaining critical metadata from the outbound side.
Query Attributes:

- Keys: Joined on Case_ID.

- Dimensions: Seller_ID, Agent_ID, Reason_Code (RC), Outbound_Message.

- Feature Engineering: Developed a Keyword Classification Logic using CASE statements and LIKE operators to categorize raw outbound messages into specific error types (e.g., "Missed Document," "Incorrect Standard," "Policy Misinterpretation").

  ### _SQL pseuodo code_
 ```sql
[SELECT 
    o.Seller_ID, 
    o.Agent_ID, 
    o.Reason_Code,
    e.Outbound_Message, -- Included for validation
    CASE 
        WHEN LOWER(e.Outbound_Message) LIKE '%missing%doc%' THEN 'Missed Document'
        WHEN LOWER(e.Outbound_Message) LIKE '%standard%' THEN 'Incorrect Standard'
        ELSE 'Other' 
    END AS Error_Category
FROM Outbound_Logs o
RIGHT JOIN Error_Audit e 
    ON o.Case_ID = e.Case_ID;]
```



  ## 📊 Dashboard & Visualization Strategy
  
I developed a weekly monitoring dashboard to drive accountability across three levels:

- The Error Funnel: A funnel chart visualizing the "leakage" points—identifying which specific error categories occurred most frequently.

- Vendor Impact (Pie Chart): Highlighted the Top Sellers whose cases were being incorrectly rejected, preventing unnecessary vendor escalations.

- Agent Performance (Bar Chart): Pinpointed specific Agent IDs with high error densities, moving away from "blanket training" to "targeted coaching."

  ## 🔍 Root Cause & Actionable Insights
  
- SOP Clarity: Identified "Grey Zones" in the existing documentation where language was ambiguous, leading to a global SOP Revision.

- Targeted Training: Discovered that 40% of errors were concentrated within one specific team, leading to a 2-week intensive retraining program.

- Seller Protection: Rectified a recurring "Missed Document" error for high-volume sellers that was caused by a UI glitch rather than agent negligence.

## 📈 Business Impact

- Error Reduction: Achieved a 10% decrease in weekly quality errors within the first month of implementation by conducting regular training sessions and making SOP changes.

- Operational Accuracy: Improved the reliability of the outbound audit trail, leading to higher stakeholder trust.

- Efficiency: Automated the weekly reporting flow, reducing manual data prep time by ~2 hours/week.

