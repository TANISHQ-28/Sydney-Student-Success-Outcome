# Sydney Student Success Challenge

This is my project for the [UTS MDSI Kaggle competition](https://www.kaggle.com/competitions/uts-mdsi-competition-2026). The task was to predict which students end up with a successful outcome (study + employability). It's scored with ROC-AUC.

Made by Tanishq Soni

## How it went

| | ROC-AUC |
|---|---|
| My cross-validation score | 0.978 |
| Public leaderboard | 0.98068 |
| Final (private) leaderboard | 0.98164,|

I only made one submission, so I didn't tune anything against the public leaderboard.

## What I did

The data is split over 6 tables (monthly records are about 280k rows), so I used Python with pandas and scikit-learn instead of Excel. Steps:

1. **Cleaning.** The data was messy on purpose. Countries were spelled many different ways, some months were duplicated, one suburb ID didn't exist, and some commute times and LinkedIn counts were way too big (10x and 20x). I fixed what I could and added a flag column so the model knows which ones were wrong.
2. **Joining.** I summed up each student's monthly rows (average study hours, total networking events, etc.) and joined everything into one table.
3. **Model.** Gradient boosting with lots of very small trees. Smaller trees gave a better score than bigger ones, which surprised me.
4. **Checking.** 5-fold cross-validation. I submitted probabilities instead of 0/1 because ROC-AUC ranks students.

## What I noticed in the data

- Internships matter the most. Success was about 35% with none, 94% with one and almost 100% with two or more.
- Success doesn't go down slowly with work hours, it drops at certain points. For students with no internships it goes 49% (up to 15 hours a week), 31% (15 to 25 hours), then 2% (over 25 hours). Commute works the same way after 60 minutes.
- LinkedIn connections, attendance, study hours and wellbeing also help. Coffee choice, phone provider and gender didn't.

## A mistake I made

At first I capped attendance and wellbeing at 100 and removed negative values because I thought they were errors. Turns out they were normal values and my score went down. After that I only fixed numbers that were wrong in a clear pattern.

## What I couldn't do

The top people scored about 0.003 higher than me. I tried other models, blending a few together and adding more features, but I couldn't close the gap, so I'm not sure what they did differently. Also, this is just patterns in competition data, not proof of what causes success.

## Running it

1. Download the data from the competition page (it's not in this repo).
2. Put the CSV files in a folder called `data/` next to the notebook.
3. `pip install -r requirements.txt`
4. Open `sydney_student_success.ipynb` and run all the cells.

## Files

- `sydney_student_success.ipynb` is the whole project with explanations
- `requirements.txt` lists the libraries
