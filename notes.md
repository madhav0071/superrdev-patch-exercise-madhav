# Notes

## What I changed

I focused on three issues that I felt had the biggest impact.

1. **Task filtering:** I found an issue in `TaskRepository.java` where the search and status conditions were not grouped correctly. Because of SQL `AND`/`OR` precedence, searching with a status could return tasks with a different status. I reproduced this by calling the API with `q=api&status=DONE` and getting an `OPEN` task. I fixed the query by grouping the title/description conditions with parentheses.

2. **Unnecessary request delay:** In `TaskController.java`, every request could be delayed by `Thread.sleep()` based on the search query length. This was unnecessary and blocked the request thread, so I removed the delay. I kept the complexity calculation because it is still used for logging.

3. **Loading state on API errors:** In `useTasks.js`, an API error set the error message but did not set `loading` back to false. This could leave the UI showing the loading state after a failed request. I fixed the error handler and also clear an old error when a new request starts.

## What I did not change

I did not try to fix every issue I noticed. I focused on bugs that affected correctness, performance, or user-visible behavior and avoided larger refactors because of the 90-minute time limit.

## Biggest remaining risk

The application has limited automated test coverage, so changes to filtering, pagination, or API behavior could introduce regressions that are not caught automatically.

## Tools used

I used VS Code and PowerShell to run and test the application. I also used ChatGPT to help understand unfamiliar parts of the React and Spring Boot code and to discuss possible fixes. I reviewed and tested the changes myself before applying them.
