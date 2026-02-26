---
description: Run all tests
---

Run *all* frontend, backend, and iOS tests as well as the iOS app build to verify they are "green and clean" i.e. no failures or warnings. Any failures or warnings must be addressed properly - this means revising the implemnetation, or in the case where the test no longer accurately reflects the state of the project, update the test. Under no circumstances should you short circuit tests or implement hacks to workaround failures. No half-assed measures. For pytest, use the .venv in the /backend folder.