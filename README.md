# [cite_start]Fitness Master (健身达人) [cite: 893]

[cite_start]Fitness Master was developed to help gym beginners overcome the steep learning curve associated with complex equipment[cite: 893]. [cite_start]By integrating Computer Vision into a mobile-first WeChat Mini-program, this platform provides real-time guidance directly within the gym environment[cite: 894, 895].

[cite_start]![Figure 1: Homepage UI and Layout Overview](./images/image1.png) [cite: 896]
*Figure 1: Homepage UI and Layout Overview*

[cite_start]Users can identify machines by taking a photo, with the backend using a custom YOLOv8 model to return instant usage instructions and safety tips[cite: 897]. [cite_start]To encourage long-term engagement, the system includes a community forum where users can share logs and interact in specialized "Bars"[cite: 898].

[cite_start]![Figure 2: AI Equipment Recognition](./images/image2.png) [cite: 899]
*Figure 2: AI Equipment Recognition*

[cite_start]![Figure 3: Workflow Diagram](./images/image3.png) [cite: 900]
*Figure 3: Fitness Master Workflow Diagram*

---

## [cite_start] System Architecture & Tech Stack [cite: 901]

[cite_start]The system utilizes a decoupled three-tier architecture consisting of a mobile client, a business logic backend, and an AI inference server[cite: 902]. 

[cite_start]![Figure 4: MINA Framework Architecture](./images/image4.png) [cite: 903]
*Figure 4: MINA Framework Architecture*

[cite_start]The backend is split between two frameworks to optimize performance[cite: 904]:
* [cite_start]**ThinkPHP 5**: Handles core BBS features, user authentication via WeChat OpenID, and all CRUD operations for the MySQL database[cite: 905].
* [cite_start]**Flask**: A lightweight Python server deployed specifically to run the YOLOv8 inference[cite: 906].

[cite_start]This isolation ensures that compute-intensive image processing tasks do not bottleneck the responsiveness of the social features[cite: 907, 917]. [cite_start]All structured data persists in a centralized MySQL database[cite: 911, 918].

---

## [cite_start]AI Logic: The Engineering Challenge [cite: 919]

[cite_start]Developing the recognition engine required a significant focus on data integrity, starting with a collection of 5,000 raw images[cite: 920, 921]. [cite_start]After a rigorous manual cleaning process, 3,300 high-quality samples were retained and precisely annotated using Labelme to ensure the model could generalize across various gym environments[cite: 922, 923].

[cite_start]![Figure 5: Visualization of Detection Results](./images/image5.jpg) [cite: 924]
*Figure 5: Visualization of Detection Results*

### Model Architecture & Training
[cite_start]I opted for the YOLOv8 architecture for its sophisticated feature extraction capabilities[cite: 926]. [cite_start]The model leverages a **C2f structure** (Cross Stage Partial Bottleneck) to enhance gradient flow, coupled with an **Anchor-Free detection head**[cite: 927]. [cite_start]Unlike traditional anchor-based models, this approach identifies object centers directly, providing more accurate predictions across varied scales and orientations[cite: 928].

[cite_start]![Figure 6: Training Loss Curves](./images/image6.png) [cite: 925]
*Figure 6: Training Loss Curves*

[cite_start]After training for 100 epochs, the model’s reliability was confirmed through detailed performance metrics, including confusion matrices and PR curves[cite: 929].

| [cite_start]![Figure 7: Confusion Matrix](./images/image7.png) [cite: 930] | [cite_start]![Figure 8: PR Curve](./images/image8.png) [cite: 931] |
| :--- | :--- |
| *Figure 7: Confusion Matrix* | *Figure 8: PR Curve* |

---

## [cite_start]Database Design [cite: 932]

[cite_start]The data persistence layer is built on a highly normalized relational schema with five core tables: **Users**, **Bars**, **Forums**, **Posts**, and **Comments**[cite: 933, 934]. 

* [cite_start]**Hierarchy**: The Forums table acts as the top-level category (e.g., Gym Discussion), maintaining a 1:N relationship with multiple Bars[cite: 935].
* [cite_start]**Relationships**: Each Bar serves as a container for Posts, which are linked via foreign keys to both their parent Bar and the original author[cite: 936].
* [cite_start]**Interaction**: The Comments table enables interactive threading by anchoring responses to specific Post and User IDs[cite: 937].

---

## [cite_start] Health Algorithms [cite: 939]

[cite_start]The system accounts for biological differences in energy expenditure by utilizing the Harris-Benedict equation to estimate a user's BMR[cite: 940]:

* [cite_start]**Male**: $$BMR = 13.7 \times weight(kg) + 5.0 \times height(cm) - 6.8 \times age + 66$$ [cite: 941]
* [cite_start]**Female**: $$BMR = 9.6 \times weight(kg) + 1.8 \times height(cm) - 4.7 \times age + 655$$ [cite: 942]

[cite_start]Building on the Body Mass Index (BMI), the system calculates a refined estimate of the user's body fat percentage to assess overall health status[cite: 943]:
[cite_start]$$BodyFat = 1.2 \times BMI + 0.23 \times age - 5.4 - 10.8 \times gender$$ [cite: 944]

---

## Repository Status
[cite_start]Due to a workstation transition post-graduation, the full source code is currently archived[cite: 945]. [cite_start]This repository serves as a **Technical Walkthrough and Architectural Overview** of the project, demonstrating the integration of SOTA CV models with a full-stack SaaS ecosystem[cite: 946].
