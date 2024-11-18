# Group Creation AED Challenge: Datathon 2024
Project made by Aleix Albaiges Torres, Gabriel Fortuny Carretero, Aniol Garriga Torra and Cristina Teixidó Cruïlles 
# Overview
This project aims to improve the team formation process at Datathon FME by building a system that matches participants based on their technical skills and hackathon objectives.
Every year, Datathon FME attracts a diverse set of participants with different goals — from winning the prize to building their portfolio, learning new skills, or connecting with fellow data enthusiasts. The goal of this project is to streamline the team formation process, making it easier and more efficient for participants to find like-minded teammates.
Rather than relying on automated team assignment, our approach focuses on matching participants using a set of distance functions that evaluates the compatibility between participants. The system takes into account a wide range of personal attributes and goals, such as:


Personal Data: Includes the participant's age, university, and dietary restrictions.

Experience and Skills: This includes their programming skills (represented as a dictionary with skill levels for each language or tool), experience level and the number of hackathons they've participated in.

Interests and Preferences: Participants provide a list of their interests and challenges they're interested in, as well as their preferred role and team size.

Objectives: Their goals for the Datathon, such as prize hunting, portfolio building, learning new skills or meeting new people.

Availability: The days and times the participant is available for team collaboration.

Friendship and Team Preferences: Whether participants have friends they want to register with and their preferred team size.

By using these diverse attributes, the system can apply different distance functions to evaluate participants' compatibility in both technical and non-technical areas. These functions ensure that participants are grouped into balanced, complementary teams that align not only in terms of their skills but also in their goals, interests, and availability.

First of all, we download the json file containing all the data for the participants and we preprocess it to create a csv file with the cleaned data. The columns name, email, shirt size, dietary restriction and fun fact are not taken into account to group. This last one says all participants like music a lot, so we do not consider it discriminating. We convert to one_hot_encoding the variables that are lists. We sort the languages and create new variables, such as maturity, calculated from age and year of studies.

For the variables that are texts, we have applied similarity search in the vector space model of information retrieval. That means that for the texts of objectives, introduction, technical_project and future_excitement we have calculated the tfidf vectors, which give a weight for each word that appears in the text depending on their frequency and on the frequency in the whole corpus. Then we have created 5 variables: Tryhard, Rookie, Learner, Portfolio i Experience, which are calculated with the similarity between a query that defines a prototype of participant and the texts.

Then we have created distance functions for all the variables that we are taking into account and calculate the total distance, which is pondered with weights, which are hyperparameters. Then we can calculate a distance matrix which is used to fastly have the distance between two participants. As an heuristic approach, we have implemented the algorithm simulated annealing that randomly does changes in the groups created in a random first allocation. We always take the best solution but we take worse solutions with a probability that is pondered with a decreasing temperature. We repeat the algorithm 10 times so that the inicialization is different. All of this are ways of scaping bad local minima.

Finally, we have created a streamlit visualization that does all the proceeding. If you are an organizer you can upload the json file with the participants and the program does the groups. IF you are a participant that has registered in the file then you can search for which is your group.

To run the project from this github repository, it is enough to run the files in the [Scripts_def](Scripts_def/) folder in this order 1. text_indexing.py, 2. text_processing_final.py, 3. Preprocessing.py and 4. cluster.optimization.py. The folder [Streamlit](Streamli/) contains the codes to run the streamlit application. The command to run it is just streamlit run home.py. The rest of folders contain jupyter notebooks which do the same as the files in the folder [Scripts_def](Scripts_def/), explained in detail and step-by-step. This notebooks are what we have used to program this project.

See more details on [Figma presentation slides](https://www.figma.com/slides/zPovvst4LEnUG9MXlnL2Pg/DATATHON2024_presentation?t=ZHP3hTtY3zJTrTBT-1)


# Prerequisites  
This project is mainly build in Python, you can install it in the following link:

Install [Python](https://www.python.org/downloads/) (3.10 or higher)

Also, also various packages are used in our implementation that can be installed with the following command

```bash
pip install -r requirements.txt
```
