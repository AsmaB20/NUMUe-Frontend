# NUMUe Frontend Function Review

This file contains a numbered function table extracted from the built frontend bundle.

| # | Function | Input | Output | Control | Mechanism | Sub functions / helpers | Linkage | Issues / Improvements |
|---|---|---|---|---|---|---|---|---|
| 1 | `l` (57117) | `e` teacherId, `s` params | API response data | async HTTP GET | `axios.get("insights/teacher/" + e, { headers, params })` | `a.sK()` token retrieval | links to 6 | No explicit error handling; centralize auth/interceptors and handle missing token.
| 2 | `i` (57117) | `e` studentId, `s` params | API response data | async HTTP GET | `axios.get("insights/student/" + e, { headers, params })` | `a.sK()` | links to 6 | Same auth/token risk; unify with teacher insights wrapper.
| 3 | `l` (88667) | `e` studentId | `achievements` array | async HTTP GET | `axios.get("students/" + e + "/achievements", headers)` | `a.sK()` | links to 11, 12 | Add fallback for missing auth and centralize error handling.
| 4 | `i` (88667) | `e` studentId | `skills` array | async HTTP GET | `axios.get("students/" + e + "/skills", headers)` | `a.sK()` | links to 12 | Add fallback for missing auth and centralize error handling.
| 5 | `a` (2812) | `e` date string, `s` bool, `t` bool | formatted date string | string split + lookup | `e.split("-")`, `parseInt`, translation keys | `n.ag.t(...)` | links to 7 | Input validation missing; use a robust date utility to avoid invalid output.
| 6 | `w` (main page hook) | `e` studentId, `s` sort/filter | query result + `assignments` | React Query + auth redirect | `useQuery({ queryFn: _O(e,s) })` | `N.useRouter()`, `j.NL()` | links to 11, 12 | 401 handling okay but should be abstracted into a common auth flow.
| 7 | `L` component | `{ assignment }` | JSX assignment card | conditional render + sanitization | `dangerouslySetInnerHTML` after sanitize | `P.default.sanitize`, `I.N`, `D.z` | links to 11, 12 | HTML truncation risks; truncate plain text before sanitization.
| 8 | `q` component | `{ assignment }` | JSX open-practice card | render summary card | `dangerouslySetInnerHTML` after sanitize | `P.default.sanitize` | links to 11 | Same HTML truncation risk; avoid slicing sanitized HTML.
| 9 | `$` hook | `e` options | query result + `assignments` | React Query with router/auth | `useQuery({ queryFn: b.Oh(e) })` | `N.useRouter()`, `j.NL()` | links to 11 | Uses debug `console.log`; remove in production.
| 10 | `K` mutation hook | `{ studentID, assignmentID }` | mutation object | React Query mutation | `mutationFn: b.Wb(s,t)` | `useMutation` | links to 11 | Add explicit mutate error handling and messaging.
| 11 | `B` component | props from `H` | assignments list UI | render list + mutate on click | sort/map assignments | `q`, `L`, `K`, `W` | links to 6, 7, 8, 9, 10, 12 | Optimize sorting to avoid repeated work and confirm stable keys.
| 12 | `H` component | none | student home dashboard | state + effect + query | hooks, callbacks, effects | `useEffect`, `useCallback` | links to 6, 7, 11, 12, 13 | State naming is dense; simplify and reduce repeated `useEffect` patterns.
| 13 | `eZ` onboarding / page entry | none | onboarding UI | localStorage flags + joyride | `react-joyride` steps/callback | `eb`, `ew` | links to 12 | Uses `window.location` and DOM manipulation; ensure safe SSR/hydration handling.

