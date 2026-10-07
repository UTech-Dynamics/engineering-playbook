# Software Industry Vocabulary Guide

A practical glossary of terms and phrases commonly used in software companies, covering project management, QA, development, and Agile teams. Each term includes a plain-language interpretation.

---

## Table of Contents

1. [Project Planning & Delivery](#1-project-planning--delivery)
2. [Requirements](#2-requirements)
3. [Task Management](#3-task-management)
4. [Frontend / Backend & Technical Terms](#4-frontend--backend--technical-terms)
5. [QA / Testing](#5-qa--testing)
6. [Risk & Problem-Solving](#6-risk--problem-solving)
7. [Scope, Client & Stakeholder Language](#7-scope-client--stakeholder-language)
8. [Communication Phrases](#8-communication-phrases)
9. [Agile / Scrum](#9-agile--scrum)
10. [Common Phrases That Sound Harder Than They Are](#10-common-phrases-that-sound-harder-than-they-are)
11. [Scenario Reasoning Practice](#11-scenario-reasoning-practice)

---

## 1. Project Planning & Delivery

| Term / Phrase | Meaning | Simple Interpretation |
|---|---|---|
| Project scope | What the project includes and does not include | "What are we actually building?" |
| Deliverable | A concrete output that must be completed | A working feature, report, or release |
| Milestone | A significant point in the project | "MVP completed" |
| Timeline | Planned schedule for completing work | When things should happen |
| Deadline | Latest time/date something must be completed | "Must be done by Friday" |
| Roadmap | High-level plan showing future work | What we plan to build and when |
| MVP | Minimum Viable Product | Smallest useful version of the product |
| Release | Making a version available to users/clients | "Release the new version" |
| Deployment | Putting software into an environment | "Deploy to production" |
| Go-live | Making a system officially operational | Client starts using it |
| Kickoff | The meeting or event that formally starts a project | "Let's align everyone before we begin" |

---

## 2. Requirements

| Term / Phrase | Meaning | Simple Interpretation |
|---|---|---|
| Requirement | Something the system must do or provide | Client's need |
| Business requirement | What the business needs to achieve | "Customers must be able to book rooms" |
| Functional requirement | Specific system behavior | "User can cancel a booking" |
| Non-functional requirement | Quality expectations such as speed, security, reliability | "The page must load in under 3 seconds" |
| Acceptance criteria | Conditions that must be satisfied for a task to be accepted | "When X happens, Y should happen" |
| User story | Requirement written from the user's perspective | "As a customer, I want to..." |
| Clarify requirements | Remove uncertainty before development | Ask what the client actually means |
| Requirement change | Client/stakeholder changes what was originally requested | New behavior is requested |
| Scope creep | Uncontrolled expansion of project scope | Small requests keep adding up |

> **Important:** If you see "acceptance criteria", think: *"How do we know this task is actually finished?"*

---

## 3. Task Management

| Term / Phrase | Meaning |
|---|---|
| Backlog | List of work that may need to be done |
| Task | A specific piece of work |
| Subtask | Smaller piece of a larger task |
| Assignee | Person responsible for doing the task |
| Priority | How important/urgent the task is |
| Dependency | One task relies on another task |
| Blocker | Something preventing progress |
| Estimate | Expected amount of effort/time |
| Workload | Amount of work assigned to someone |
| Capacity | How much work a person/team can realistically handle |
| Task status | Current state: To Do, In Progress, Review, Done, etc. |
| Handoff | Passing work from one person/team to another |
| Follow-up | Checking whether an action was completed |
| Definition of Done (DoD) | Team-wide checklist that defines when any work item counts as complete |

**Example:**
"The frontend task is blocked by the API."
**Means:** Frontend cannot proceed properly until the backend API is available.

---

## 4. Frontend / Backend & Technical Terms

| Phrase | Meaning |
|---|---|
| Frontend | User-facing interface |
| Backend | Server-side logic, APIs, database, authentication, etc. |
| API | A way for software components to communicate with each other |
| API integration | Frontend (or another system) communicating with the backend/API |
| API endpoint | Specific API URL/function providing data or accepting actions |
| Backend dependency | Frontend work depends on backend functionality |
| Technical dependency | One technical component depends on another |
| Database change | Modification to database structure/data |
| Business logic | Rules determining how the application behaves |
| Third-party integration | Connecting an external service/API |
| Environment | A setup such as development, staging, or production |
| Staging | Environment used for testing before production |
| Production | Live system used by real users |
| Version control | System (such as Git) that tracks changes to code |
| Pull request (PR) | A request to review and merge code changes |
| Code review | Teammates check code quality before it is merged |
| CI/CD | Automated building, testing, and deploying of code |
| Hotfix | Urgent fix released outside the normal schedule |
| Rollback | Reverting to a previous working version |
| Regression | Previously working functionality becomes broken after a change |
| Technical debt | Existing technical shortcuts/problems that make future work harder |
| Refactoring | Improving internal code without changing intended behavior |

---

## 5. QA / Testing

| Term | Meaning |
|---|---|
| QA | Quality Assurance |
| Test case | Specific scenario used to test functionality |
| Bug / Defect | Something that does not work as expected |
| Bug reproduction | Steps needed to make the bug happen again |
| Regression testing | Checking that new changes didn't break existing features |
| Smoke testing | Quick check that the major system functions work |
| UAT | User Acceptance Testing (final validation by client or end users) |
| Test environment | Environment where testing is performed |
| Bug severity | How seriously a bug affects the system |
| Bug priority | How urgently the bug should be fixed |
| QA sign-off | QA confirms the feature is ready |

> **Important distinction:** Severity is not the same as Priority.
> A bug can be very severe but low priority if it affects a rarely used feature.

---

## 6. Risk & Problem-Solving

| Phrase | Meaning |
|---|---|
| Project risk | Something that could negatively affect the project |
| Risk mitigation | Action taken to reduce a risk |
| Risk assessment | Evaluating likelihood and impact of a risk |
| Blocker | Immediate issue stopping progress |
| Issue | A problem that has already occurred |
| Escalate | Bring a problem to someone with authority to resolve it |
| Root cause | The underlying reason a problem happened |
| Impact | Effect of a problem/change |
| Contingency plan | Backup plan if something goes wrong |
| Bottleneck | Part of the process limiting overall progress |

**A useful distinction:**

- **Risk:** "The API developer may not finish on time."
- **Issue:** "The API developer has missed the deadline."
- **Blocker:** "The frontend cannot continue because the API is unavailable."

---

## 7. Scope, Client & Stakeholder Language

**Stakeholder**
Anyone with an interest in or influence over the project.
Examples: client, company owner, developer, QA engineer, designer.

**Stakeholder alignment**
Making sure everyone has the same understanding of goals, scope, and expectations.

**Client expectation**
What the client believes will be delivered.

**Scope change**
A change to the agreed project requirements.

**Change request**
A formal request to modify the agreed scope.

**Impact assessment**
Checking how a change affects:

- Timeline
- Cost
- Resources
- Technical work
- Existing functionality

> **Key phrase:** "Let's assess the impact before committing to the change."
>
> **Meaning:** Don't immediately say yes. First determine what the change will cost in time and resources.

---

## 8. Communication Phrases

| Phrase | Meaning |
|---|---|
| Progress update | What has been completed and what is currently happening |
| Status update | Overall condition of the project |
| Blocker update | Something preventing progress and what is being done about it |
| Action item | A specific action someone needs to take |
| Owner | Person responsible for an action/task |
| ETA | Estimated Time of Arrival: expected completion time |
| FYI | For Your Information: no immediate action required |
| ASAP | As Soon As Possible |
| Follow-up | Checking progress after a previous discussion/action |
| Heads-up | An early warning about something that may affect others |
| Circle back | Return to a topic later |
| Loop in | Include someone in the conversation |
| Sync | A meeting or conversation to get aligned |

**Example:**
"What's the ETA for the API?"
**Means:** "When do you expect the API to be ready?"

---

## 9. Agile / Scrum

You don't need to be a Scrum expert, but you should recognize these terms.

| Term | Meaning |
|---|---|
| Agile | Iterative approach to software development |
| Sprint | Short, fixed development period (often 1-4 weeks) |
| Sprint planning | Decide what the team will work on during a sprint |
| Sprint backlog | Work selected for the current sprint |
| Daily stand-up | Short team progress/blocker meeting |
| Sprint review / demo | Team shows completed work to stakeholders |
| Retrospective | Team discusses what went well and what should improve |
| Backlog refinement | Clarifying and preparing future work |
| Product backlog | Ordered list of product work |
| Epic | Large body of related work |
| Feature | A user-visible capability |
| Story points | Relative estimate of effort/complexity |
| Velocity | Amount of work a team typically completes per sprint |
| Kanban | Visual workflow board focused on continuous flow |
| Scrum Master | Person who helps the team follow Scrum and removes obstacles |
| Product Owner | Person who decides what the product should deliver and in what order |

**Work breakdown hierarchy:**

Epic → Feature / User Story → Task → Subtask

Think: Large → smaller → actionable → very specific.

---

## 10. Common Phrases That Sound Harder Than They Are

| Phrase | Plain Meaning |
|---|---|
| "Keep the project on track" | Make sure work is progressing toward the agreed deadline/scope |
| "Manage competing priorities" | Decide which work should be done first when everything cannot be done simultaneously |
| "Cross-functional collaboration" | Different roles working together, e.g. developers + QA + designers + client |
| "Resource allocation" | Decide who should work on what |
| "Balance effort and impact" | Consider both how difficult something is and how valuable it is |
| "Identify dependencies" | Find out what work must happen before other work can proceed |
| "Proactively identify risks" | Look for possible problems before they happen |
| "Drive delivery" | Actively make sure the project moves toward completion |
| "Remove blockers" | Resolve or coordinate around things preventing progress |
| "Ensure alignment" | Make sure everyone understands and agrees on the same goal/plan |
| "Manage expectations" | Make sure clients/team members have realistic expectations about what can be delivered |
| "Take ownership" | Don't merely report a problem; take responsibility for moving it toward resolution |
| "End-to-end ownership" | Responsibility from initial requirements through development, testing, deployment, and delivery |

---
