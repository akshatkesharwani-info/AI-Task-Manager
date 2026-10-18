# AI Task Manager — Add Tasks, Get AI Priorities, Deadlines and a Daily Plan

A Colab notebook that keeps a task list with deadlines and effort, works out a rule-based priority in code, asks an AI for its own priority, order and a suggestion, and builds a plan for the hours you have today.

## Problem

A to-do list does not tell you what to do first.

## Approach

1. Keep tasks in a pandas table: id, task, deadline, hours, status. Sample deadlines are set from today's date. Saved tasks are reloaded from `tasks.csv` if the file exists
2. Add your own tasks (name, deadline as YYYY-MM-DD, hours)
3. Rule priority in code: due today or overdue is High. Due within 3 days is Medium, or High if it needs 4+ hours and is due within 2 days. Later is Low
4. Ask an LLM on Groq (`openai/gpt-oss-120b`) for a priority, an order and a reason for every task, plus one short suggestion, as JSON. If the reply cannot be read, the rule priority is used
5. Ask for a simple plan for the hours you have today
6. Mark tasks done and save to `tasks.csv`

## Tech Stack

Python, pandas, Groq API (OpenAI-compatible client), Google Colab

## Results

One real run on the 6 sample tasks (today was 2026-10-17).

| Task | Due in | Rule | AI |
|---|---|---|---|
| Workout | today | High | High |
| Complete project report (4 h) | 1 day | High | High |
| Pay electricity bill (0.5 h) | 2 days | Medium | High |
| Prepare for interview (5 h) | 3 days | Medium | High |
| Plan weekend trip (1 h) | 5 days | Low | Medium |
| Learn AI tools (3 h) | 10 days | Low | Low |

- The AI agreed with the rule on 3 of 6 tasks and raised the other 3. It gave a reason for each, for example the interview needs 5 hours of preparation in 3 days
- For a 4-hour day the plan scheduled Workout (1 h), the first 2.5 h of the project report and the bill (0.5 h), and left the rest for tomorrow
- The plan has small slips: it labelled a 15-minute buffer "0.15 h", and the suggestion called the project report a "today" task though it is due tomorrow

## How to Run

1. Open `11_AI_Task_Manager.ipynb` in Google Colab
2. Get a free API key from [console.groq.com](https://console.groq.com)
3. Click Runtime → Run all, paste the key when asked
4. Add your own tasks, or press Enter to keep the samples
5. Read the priorities, enter your free hours, and download `tasks.csv` at the end (Colab clears files when the session ends)

## Limitations

- Tested only with the 6 sample tasks. Adding tasks, marking tasks done and reloading a saved file were not tested in my run
- The AI fallback to the rule priority did not happen in my run, so it is untested live
- The AI daily plan can have small mistakes, so read it before following it
- Priorities are advice, not a guarantee. The AI cannot know things outside the table, such as how important a task really is to you
- It runs as a notebook only and is not deployed

## Next Steps

- Test adding, completing and reloading tasks
- Recurring tasks and calendar sync
- Reminders by email
- Streamlit app with drag and drop

## Author

**Akshat Kesharwani** | [GitHub](https://github.com/akshatkesharwani-info) | [Portfolio](https://akshatkesharwani-info.github.io/)
