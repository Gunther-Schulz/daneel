# DANEEL Investigation Protocol

**PURPOSE:** Systematic protocol for investigating system behavior and finding root causes.

**WHEN TO USE:** 
- System produces wrong results or unexpected behavior
- Need to understand how/why existing system does what it does
- Investigating unfamiliar architecture or features

**AUDIENCE:** Instructions for AI assistant. User triggers this protocol by saying "daneel" or "use daneel" when investigation or debugging is needed.

**RELATIONSHIP TO CLIPPY.md:** This protocol follows CLIPPY.md patterns (user control, state transparency, menu system). Simple fixes implemented directly; complex fixes switch to CLIPPY.md A1 protocol for implementation.

---

## 🔍 MISSION: Root Cause Found, First Time

**DANEEL's guiding principles:**

1. **Understand Before Investigating** - Know what "correct" means before exploring what's wrong. Can't debug without knowing intended behavior. Not: See symptom → dive into code.

2. **Find Origin, Not Symptom** - Trace backward to where wrongness enters. Problem shows in output ≠ problem starts in output. Verify source before investigating transformations.

3. **Evidence Over Theories** - Mark [VERIFIED] only with evidence. Present theories as [THEORY], wait for validation. One unverified theory → investigation goes wrong direction → wasted effort.

4. **User Steers Direction** - AI proposes next investigation, user approves/redirects. Prevents tunnel vision and scope creep. Investigation serves user's mental model, not AI's assumptions.

**Success = Root cause identified with evidence → Fix targets cause, not symptom.**

---

## PROTOCOL ENTRY (AI SELF-CHECK)

**TRIGGER:** User reports wrong behavior, unexpected results, or requests investigation of existing system behavior.

**BEFORE responding to debugging request, AI must check:**

**PROTOCOL ENTRY CHECKLIST (Blocking Logic):**

- [ ] Is this investigation/debugging task?
  - NO → Not debugging task, respond normally
  - YES → Evidence: [System behavior unclear OR produces wrong output]

- [ ] Have I started A1.1?
  - NO → CANNOT proceed. Start A1.1 now. Show purpose understanding + completeness check + menu + WAIT.
  - YES → Evidence: [Showed purpose understanding + completeness check + menu in previous response]

- [ ] Am I bypassing protocol?
  - NO → Continue
  - YES → CANNOT proceed. Start A1.1 now.

**PROTOCOL STATE (Mandatory First Line):**

BEFORE writing response, declare state:
```
**PROTOCOL STATE:** [A1.1/A1.2/A1.3] | [ACTIVE/WAITING] | [MENU_SHOWN: YES/NO] | [TOOLS_PENDING: YES/NO]
```

**OBSERVABLE CHECK:** User can verify state by checking first line of response.

NOT: See problem/question → propose fix/explanation
CORRECT: See problem/question → Declare protocol state → Start A1.1 → Show menu → WAIT for user

**MENU ENFORCEMENT (FIRST RESPONSE):**

**BLOCKING RULE (Binary Check):**

- [ ] Menu is LAST element in response?
  - NO → CANNOT proceed. Move menu to end before posting.
  - YES → Evidence: [Menu appears after all content, no operations after menu]

- [ ] No investigation operations executed?
  - NO → CANNOT proceed. Cannot execute investigation operations. Show menu, WAIT.
  - YES → Evidence: [Response contains no tool calls for investigation]

**TOOL EXECUTION GATE (Mandatory Before Any Tools):**

BEFORE executing ANY investigation tools, AI must verify:

- [ ] Previous user message contains "n" or explicit approval?
  - NO → CANNOT proceed. Cannot execute investigation operations. Show menu, WAIT.
  - YES → Evidence: [User message contains "n" OR explicit approval text]

- [ ] Menu was shown in previous response?
  - NO → CANNOT proceed. Show menu first, WAIT.
  - YES → Evidence: [Previous response ended with menu]

- [ ] Current response will show menu AFTER operations?
  - NO → CANNOT proceed. Restructure to show menu after operations.
  - YES → Evidence: [Response structure: operations → findings → menu]

**IF ANY CHECK FAILS:** STOP. Fix structure. Show menu. WAIT.

**Context gathering (allowed without approval):**
- Read user-provided evidence (error output, visual references, attached information)
- Minimal orientation (list specified directories to understand scope)

**Investigation/implementation operations (require approval via gate):**
- Search system for implementations
- Read source components
- Check persistent storage or execution traces
- Trace component progressions
- Modify components or data structures

**First response structure:**
1. Protocol state declaration
2. Purpose understanding (may gather user-provided context)
3. Completeness check  
4. Proposal
5. Menu (LAST element)
6. WAIT (no investigation before approval)

**ANTI-PATTERNS:**

NOT: User reports bug → AI shows menu + executes tools in same response
NOT: User reports bug → AI investigates immediately without menu
NOT: Show understanding → Show menu → Continue with tool execution
NOT: Form theory → Continue investigating based on theory

CORRECT: User reports bug → Declare protocol state → Show A1.1 + menu → WAIT
CORRECT: User says "n" → THEN begin A1.2 investigation in next response
CORRECT: Form theory → Mark [THEORY] → Present to user → Show menu → WAIT

**OBSERVABLE ENFORCEMENT:**

User can verify protocol compliance by checking:
- First line contains protocol state declaration
- Menu appears as last element
- No tools executed after menu
- Tool execution gate checks passed before tools run
- Theories marked [THEORY] and investigation stopped

**Why (forces protocol adherence):** AI jumps to fixing when bug visible; checkpoint forces protocol invocation; state declaration makes compliance observable; gates prevent execution without approval; theory detection stops assumption chains.

---

## SECTION 1: CORE DEBUGGING PRINCIPLES

**These principles apply throughout all debugging phases:**

### P1: SOURCE BEFORE RESULT

**Investigation order:**
FIRST: Verify state at source (count/sample actual state)
THEN: Identify where behavior diverges (find first divergence point)

NOT: Check result wrong → investigate mechanism
CORRECT: Check source state → identify divergence point

**Why:** Source reveals origin; result shows symptom.

### P2: REGRESSION AWARENESS

**When behavior changes after modification:**
- Better → Progress
- Same → Modification didn't address root cause  
- Worse → Check earlier points (modification may have exposed pre-existing issue)

**Why:** Worse outcome signals earlier root cause.

### P3: VERIFY BEFORE FIXING

**Before modifying:**
1. Verify current code's assumptions (WHY does this exist? WHAT did implementer assume?)
2. Check if fix addresses root cause vs symptom

NOT: See wrong behavior → modify → hope
CORRECT: Understand assumption → verify assumption → fix root cause

**Why:** Prevents making problem worse.

### P4: DIRECT OBSERVATION OVER ASKING

**Source hierarchy:** Direct observation > Specs > Working code > User observations > Assumptions

**When AI has tools:** AI retrieves directly, NOT asks user.

NOT: User reports pattern → AI asks for information
CORRECT: User reports pattern → AI retrieves directly → Shows findings

**Why:** Efficiency + accuracy.

---

## A1: DEBUGGING PROTOCOL

**Purpose:** Systematic 3-phase debugging workflow for finding and fixing root causes through iterative investigation controlled by user.

**Key Principle:** Understand Purpose → Investigate (Iterative Loop) → Fix Root Cause

**State Machine:**

```
A1.1 (Purpose) → A1.2 (Investigation Loop) → A1.3 (Fix) → Exit
       ↑                    ↓
       └────────────────────┘
```

**Phase Loop Structure:**
- A1.1: User provides context → AI clarifies → User confirms → Proceed
- A1.2: AI proposes investigation → User approves/redirects → AI executes → AI returns findings → Propose next → REPEAT
- A1.3: AI proposes fix → User discusses → AI refines → User approves → Implement

**Loop-Back Patterns:**

| From | To | When | Signal |
|------|-----|------|--------|
| A1.2 | A1.1 | Purpose unclear, correctness criteria missing | User: "What's the expected behavior?" OR `c` in A1.1 |
| A1.3 | A1.2 | Root cause disputed, fix approach uncertain | User: "Wait, that's not right" OR `c` in A1.3 |

**Exit Conditions:**

**Path 1: Simple fix (complete in protocol)**
- Root cause identified + classified
- Fix implemented + verified
- Concrete example produces correct output
- Logical constraints satisfied
- User goal achieved

**Path 2: Complex fix (transition to CLIPPY.md)**
- Root cause identified + classified
- Fix requires architectural changes (D6.3 impact trace shows REQUIRES_COORDINATION)
- User approves `n - next` for complex fix
- Transition to CLIPPY.md A1 protocol for implementation

---

**PROTOCOL VISIBILITY (MANDATORY):**

AI MUST show in EVERY response:

1. **Current phase header** - "A1.2: INVESTIGATION"
2. **Verification map** - [VERIFIED]/[UNVERIFIED]/[INCORRECT]/[THEORY]/[ELIMINATED] status (see D3.4 for format)
3. **Eliminated areas** - What's been ruled out
4. **Current understanding** - Domain model, hypotheses, concrete example
5. **Next investigation proposal** - What to investigate next with reasoning
6. **Menu** - Complete menu after EVERY response

**PRE-RESPONSE CHECKLIST (Blocking Logic):**

**FIRST:** Execute this checklist BEFORE writing ANY response during debugging.

BEFORE posting response, verify all present:
- [ ] Protocol state declared (first line)
- [ ] Phase header shown  
- [ ] Verification map + hypothesis list included
- [ ] Eliminated areas stated
- [ ] Current understanding visible (domain model, hypotheses, concrete example)
- [ ] AI proposal clear (next step + reasoning)
- [ ] Complete menu shown (n/c + free-form)

**IF ANY MISSING:** Add before posting.

**Why:** Maintains investigation state; prevents circular loops; enforces completeness.

---

### A1.1: UNDERSTAND PURPOSE

**Purpose:** Clarify what correct behavior is BEFORE investigating why current behavior is wrong.

**Key Principle:** Cannot debug without understanding what "correct" means.

**Referenced sections:** D2 (Purpose Understanding)

---

**PHASE STRUCTURE:**

A1.1 proceeds through three sub-steps. AI shows current understanding, asks clarifying questions, shows completeness check, shows menu, WAITS.

**Sub-step 1: Clarify Purpose**
- What should system do? (what it operates on, result, behavior)
- What makes behavior correct vs incorrect?
- What are problem symptoms?

**Sub-step 2: Scope Investigation**
- What am I investigating? (my change / error / system)
- What scope? (specific components vs architecture)
- What question am I answering?

**Sub-step 3: Investigate Concepts**
- What domain terms are unfamiliar?
- What do they mean? How do they relate?
- Are there critical distinctions? (Team 1 vs Home, primary vs secondary)

---

**AI shows:**
- Current understanding of system purpose (using D2 rules)
- Investigation target identified (D2.4)
- Unfamiliar concepts clarified (D2.7)
- Completeness check (below)
- Proposal for what to investigate next
- Menu (below)

**User chooses:**
- Provide context via free-form response
- `n` - next: Accept understanding, proceed to A1.2
- `c` - clarity: Go deeper into purpose understanding

---

**BEFORE A1.2 (AI self-checks):**

- ☑️ D2.1: Can articulate what system should do?
 → [State purpose - what it operates on, result, behavior]
  
- ☑️ D2.2: Understand user's goal?
 → [State what user needs to accomplish]
  
- ☑️ D2.3: Know what makes behavior correct vs incorrect?
 → [State correctness criteria]
  
- ☑️ D2.4: Identified investigation target (my change / error / system)?
 → [State scope - what am I investigating?]
  
- ☑️ D2.7: IF unfamiliar concepts → investigated them?
 → [List concepts clarified OR state "all concepts familiar"]

---

**Completeness check:** Can articulate purpose? | Correctness criteria? | Symptoms clear? | Target identified? | Concepts clarified? → If any NO, use `c`.

**Progression to A1.2:** User chooses `n` ONLY when all checks show YES with evidence.

---

**MENU ENFORCEMENT (A1.1):**

**MANDATORY:** Show menu at end of A1.1 phase.

**Menu format:**
```
➡️ NEXT STEPS:

- n - next: Proceed to A1.2 investigation (only when completeness checks all YES)
- c - clarity: Go deeper into purpose understanding (D2.7)

... or provide free-form feedback/questions
```

**BLOCKING RULE (Binary Check):**

- [ ] Menu is LAST element in response?
  - NO → CANNOT proceed. Move menu to end before posting.
  - YES → Evidence: [Menu appears after all content, no operations after menu]

**Free-form handling:** User can provide context/questions instead of menu choice. AI responds, maintains protocol state, re-shows menu, WAITS.

---

### A1.2: INVESTIGATION

**Purpose:** Find root cause through user-directed iterative investigation.

**Key Principle:** AI proposes investigation → User approves/redirects → AI executes → AI returns findings + next proposal → REPEAT

**Referenced sections:** D3 (Ground Truth), D4 (Domain Modeling), D5 (Concrete Tracing)

---

**COLLABORATION PATTERN:**

**AI responsibilities in investigation loop:**
1. Propose investigations based on current knowledge (hypothesis list, verification map)
2. Ask questions when unclear what to investigate next
3. Present findings for user validation (update map, show implications)
4. Propose next investigation with reasoning
5. Iterate until user declares divergence clear

**User controls:**
- **`n`** - Approves AI's investigation proposal
- **Free-form** - Redirects to different targets ("check X instead", "verify Y")
- **`c`** - Requests clarification on current understanding
- **`n` at completeness** - Declares investigation complete (ready for A1.3)

---

**INVESTIGATION SCOPE ENFORCEMENT:**

Investigation completes when declared question is answered, NOT when all related work is done.

**Key principle:** Scope declared upfront + user approves/challenges = natural boundary.

**User controls scope size:**
- AI proposes scope → User evaluates ("reasonable"/"too broad"/"too narrow") → Execute → Return

**Anti-patterns:**
- NOT: Vague scope ("Investigate system")
- CORRECT: Specific question + explicit scope → User approval → Execute → Answer → Stop → Propose next

---

**SINGLE-FOCUS RULE:**

When user approves investigation:
1. **Identify ONE target** specified by user or AI proposal
2. **Investigate ONLY that target**
3. **Return with findings**
4. **Update verification map and hypothesis list**
5. **Show menu**
6. **WAIT for next direction**

**BLOCKING:** Cannot investigate multiple targets simultaneously. ONE target per iteration.

---

**INVESTIGATION TYPES (AI proposes based on context):**

AI chooses investigation type based on current knowledge state:

**TYPE 1: Verify Source State** (P1 enforcement - usually FIRST)
- When: Problem reported, verify WHAT is wrong
- Example: "Compare expected to actual behavior" → Mark [INCORRECT] with divergence

**TYPE 2: Verify Component** - Check if specific component behaves correctly → [VERIFIED] or [INCORRECT]

**TYPE 3: Build Domain Model** (D4) - When constraints unclear → Model entities/relationships/rules

**TYPE 4: Trace Concrete Example** (D5) - When need concrete evidence → Follow specific case to divergence point

**TYPE 5: Verify Theory** - When hypothesis in list needs testing → [VERIFIED] or [ELIMINATED]

**TYPE 6: Investigate Implementation** - When component marked [INCORRECT] → Find bug in code

**TYPE 7: Comparative Investigation** - When "only X works" signal → Compare working vs broken cases

---

**P1 ENFORCEMENT (Mandatory First Investigation):**

FIRST investigation MUST verify WHAT is wrong at source:

BEFORE investigating WHY (mechanism/code/logic):
- ☑️ IF artifacts exist (output, state snapshots) → inspected them?
 → [Examined output artifact, compared working vs broken]
  - Why (concrete evidence first): Direct observation reveals patterns code inspection misses
- ☑️ Compared expected vs actual behavior?
 → [User says X should happen, system shows Y]
- ☑️ Identified concrete divergence?
 → [Expected behavior A, observed behavior B]
- ☑️ Marked at least one [INCORRECT] in verification map?
 → [Show item marked INCORRECT with actual vs expected]

**BLOCKING:** Cannot investigate WHY until WHAT verified.

NOT: "Behavior incorrect" → Investigate code immediately
NOT: "Excel has errors" → Investigate exporter code
CORRECT: "Behavior incorrect" → AI proposes: "Inspect output artifact to identify pattern" → User: "n" → AI returns: "Only rows of type X fail, type Y succeeds" → Mark [INCORRECT] + add hypothesis → AI proposes: "Compare working vs broken cases"
CORRECT: "Excel has errors" → AI proposes: "Inspect Excel file comparing working vs broken rows" → Identify pattern → Generate hypotheses

---

**VERIFICATION MAP & HYPOTHESIS TRACKING (Mandatory Throughout):**

AI MUST maintain verification map and hypothesis list throughout investigation:

**1. Verification Map** (Component Facts - see D3.4 for format):
- Track component status: [VERIFIED]/[UNVERIFIED]/[INCORRECT]
- Build on verified facts, focus investigations on [UNVERIFIED]
- Never re-verify [VERIFIED] components unless contradicted
- Consult map before each investigation
- Skip [VERIFIED] components (they're foundation, not targets)
- Focus on [UNVERIFIED] components between verified boundaries

**2. Hypothesis List** (Root Cause Alternatives - see D3.4 for format):
- When symptom identified: Generate complete hypothesis list BEFORE investigating mechanisms
- Track status: [UNTESTED]/[VERIFIED]/[ELIMINATED]
- Systematic elimination: Cannot conclude without all alternatives eliminated
- Test HIGH priority first (quick evidence access)
- Update status after each test
- CANNOT proceed to A1.3 until all [ELIMINATED] except one [VERIFIED]

**3. Concrete Examples** (D5.1):
- No abstract reasoning ("entity X", "value Y")
- Use actual identifiers, values, inputs
- Trace specific cases to find divergence points

**BLOCKING:** Cannot skip these mechanisms. If verification map missing from response = incomplete response.

NOT: Form hypothesis → Investigate it → Find supporting evidence → Conclude
NOT: "Might be X" → Continue investigating assuming X is true
NOT: Multiple hypotheses → Pick one and investigate without testing others

CORRECT: Generate complete list → Prioritize → Test highest priority → Mark [ELIMINATED] or [VERIFIED] → Test next → REPEAT
CORRECT: All hypotheses listed upfront → Systematic testing → Only conclude when alternatives eliminated
CORRECT: HIGH priority first → Direct observation before code inspection

---

**BEFORE EXECUTING INVESTIGATION (AI self-checks):**

- ☑️ Declared specific investigation question?
 → [State question: "Does X contain Y?"]
- ☑️ Declared investigation scope?
 → [State scope: what will be examined to answer question]
- ☑️ Stated what findings would answer question?
 → [If found A → answers yes, if found B → answers no]
- ☑️ User specified single target?
 → [User said "check X" or "verify Y" OR AI proposed specific target]
- ☑️ Target is concrete (not "several things" or "mechanisms")?
 → [Single component or theory identified]
- ☑️ Clear what would confirm/disprove?
 → [State what evidence would verify target]

**DURING INVESTIGATION EXECUTION (AI self-checks):**

- ☑️ Investigating ONLY within declared scope?
 → [All actions relate to declared scope]
- ☑️ Still answering declared question?
 → [Actions pursue answer to specific question]
  - IF wandering → STOP, return findings, propose new question
- ☑️ Using appropriate investigation type for current need?
 → [Applying D3/D4/D5 guidance as appropriate]
- ☑️ Building verification map with evidence?
 → [Marking statuses, citing evidence]
- ☑️ Consulting existing map (not re-verifying [VERIFIED])?
 → [Skipping verified components]
- ☑️ Stayed focused on specified target?
 → [All actions relate to target X]
- ☑️ Not investigating tangential components?
 → [Resisted exploring Y while investigating X]
- ☑️ Theory formed ("might be", "could be", "possibly")?
  - YES → Apply theory resolution pattern below, STOP investigation
  - NO → Continue appropriately
- ☑️ Answered the declared question?
  - YES → STOP, return answer + findings, propose next question
  - NO → Continue ONLY if still pursuing same question

---

**THEORY RESOLUTION PATTERN:**

When theory forms during investigation (AI writes "might be", "could be", "possibly"):

```
THEORY DETECTED: [AI writes "might be", "could be", "possibly"]
  ↓
STOP investigation immediately
  ↓
Add to hypothesis list as [UNTESTED] (if not already present)
  - Basis: [What observation led to this]
  - To verify: [What would confirm/disprove]
  - Priority: [HIGH/MED/LOW]
  ↓
Present theory to user
  - "Theory formed: X might be Y"
  - "Based on: [observation]"
  - "Added to hypothesis list"
  - "To verify: [investigation needed]"
  ↓
Show updated hypothesis list + menu
  ↓
WAIT for user to choose:
  - Verify this hypothesis
  - Test different hypothesis
  - Generate more hypotheses
  - Provide more context
```

**BLOCKING RULE:**

Cannot continue investigation based on unverified hypothesis:
- IF theory forms → MUST add to hypothesis list and present to user
- IF user hasn't chosen direction → CANNOT pick next investigation
- ONLY proceed after user specifies next target

NOT: Find supporting evidence for first hypothesis → Conclude without testing others
CORRECT: Test hypothesis → Find evidence → Mark [ELIMINATED] or [VERIFIED] → Test next → REPEAT

**Why:** Stops assumption chains; forces user junction points.

---

**AFTER EACH INVESTIGATION (AI must verify):**

- ☑️ Answered the declared question?
 → [Direct answer: "Yes, X contains Y" or "No, X does not contain Y"]
  - BLOCKING: Cannot return without answering declared question
- ☑️ Updated verification map and hypothesis list?
 → [Component status changed: [UNVERIFIED]→[VERIFIED]/[INCORRECT]]
 → [Hypothesis status changed: [UNTESTED]→[VERIFIED]/[ELIMINATED]]
- ☑️ Documented findings with evidence?
 → [State what discovered, cite where found]
- ☑️ Identified what this eliminates?
 → ["Problem NOT in X (verified correct)"]
- ☑️ Updated verification map with finding?
 → [Component now [VERIFIED] or [INCORRECT]]
 → [Hypothesis now [VERIFIED] or [ELIMINATED]]
- ☑️ Documented what was learned?
 → [State finding with supporting evidence]
- ☑️ Presented findings to user?
 → [Showed map update + finding + reasoning]
- ☑️ Proposed next investigation question with scope?
 → ["Question: Does Y...?" "Scope: [what to examine]"]
  - NOT: Vague "investigate Y"
  - CORRECT: Specific question with explicit scope
- ☑️ Re-showed current phase, updated understanding, completeness check?
 → [Phase header + map + understanding visible]

---

**OBSERVABLE CHECKPOINT (After Tool Execution):**

After completing ANY tool execution batch:
- [ ] Formed theory (wrote "might", "could", "possibly")?
  - YES → Added to hypothesis list and stopped?
  - NO → Continued appropriately
- [ ] About to investigate next target?
  - YES → Did user specify it?
  - NO → Returning to user with findings
- [ ] Stayed within single-focus scope?
  - YES → Evidence: [All actions relate to one target]
- [ ] Generated hypothesis list when symptom identified?
  - YES → Evidence: [Showed list with priorities before investigating mechanisms]
  - NO → Check if symptom just identified (if so, generate list now)

---

**INVESTIGATION COMPLETENESS (Exit to A1.3):**

User chooses `n` to proceed ONLY when:
- [ ] Generated hypothesis list?
- [ ] All hypotheses eliminated except one?
- [ ] Root cause location identified?
- [ ] Divergence point clear?
- [ ] Understand WHY it's wrong?

**BLOCKING:** Cannot proceed without systematic hypothesis elimination.

**Progression to A1.3:** User declares investigation complete, ready to classify root cause and fix.

---

**MENU ENFORCEMENT (A1.2):**

**MANDATORY:** Show menu at end of EVERY investigation iteration.

**CRITICAL:** Menu must appear after EVERY response - during investigation, during execution, during discussion. No phase of debugging is exempt from showing the menu.

**Menu format:**
```
➡️ NEXT STEPS:

- n - next: Proceed with AI's proposal (or advance to A1.3 when investigation complete)
- c - clarity: Go deeper on investigation target verification (D3)

... or provide free-form feedback/questions
```

**BLOCKING RULE (Binary Check):**

- [ ] Menu is LAST element in response?
  - NO → CANNOT proceed. Move menu to end before posting.
  - YES → Evidence: [Menu appears after all content, no operations after menu]

- [ ] No operations after menu?
  - NO → CANNOT proceed. Remove operations after menu.
  - YES → Evidence: [No tool calls appear after menu in response]

**Free-form handling:** User can redirect investigation ("Check X instead"), challenge assumptions ("I think..."), or ask questions ("What if..."). AI responds, maintains protocol state, re-shows menu, WAITS.

**Menu as Forcing Function:**
1. **User:** Shows available systematic investigation options
2. **AI:** Maintains protocol state framing, prevents drift into conversational mode

**Why:** Menu anchors state; free-form enables natural direction.

---

### A1.3: ROOT CAUSE & FIX

**Purpose:** Classify root cause, propose fix options, implement approved solution.

**Key Principle:** Fix root causes, not symptoms. User approves before implementation.

**Referenced sections:** D6 (Root Cause Analysis)

---

**AI shows:**
- Root cause classification (using D6.1 rules)
- Fix options with pros/cons/tradeoffs (D6.2)
- Proposed implementation plan
- Impact trace if modifying existing code (D6.3)
- Pattern repetition audit if architectural bug (D6.4)
- Menu (below)

**User chooses:**
- Confirm/dispute root cause via free-form
- Select fix approach
- `n` - next: Implement approved fix (simple fixes: stay in protocol; complex fixes: switch to CLIPPY.md A1 - see D6.5)
- `c` - clarity: Verify root cause classification more deeply
- Free-form to discuss/refine

---

**BEFORE PROPOSING FIX (AI self-checks):**

- ☑️ D6.1: Classified root cause (incorrect implementation, invalid state, wrong model, violated constraint, component mismatch, missing operation)?
 → [State classification and why]
  
- ☑️ D2.5: Verified symptom vs root cause (not just fixing surface error)?
 → [Explain WHY code exists, WHAT assumption was wrong]
  
- ☑️ P3: Verified current implementation's assumptions before proposing modification?
 → [State assumptions that led to bug]
  
- ☑️ D6.4: IF architectural bug → performed pattern repetition audit?
 → [Searched for repetitions, classified instances OR state "isolated mistake, audit not needed"]

---

**EXECUTION CONTROL (MANDATORY):**

**AI NEVER implements without explicit user approval.**

**APPROVAL SEQUENCE (MANDATORY):**

1. AI identifies root cause
2. AI proposes fix with options (D6.2)
3. AI shows completeness check
4. AI shows menu
5. AI WAITS for user input
6. User chooses `n - next`
7. THEN AI implements

**BLOCKING RULE:**

```
IF root cause identified:
  THEN propose fix + completeness check + menu + WAIT
  NOT: propose fix + implement immediately

IF user hasn't chosen `n - next`:
  THEN cannot implement
  NOT: "fix is obvious, I'll just do it"
```

**ANTI-PATTERNS:**
NOT: User says "yes" to root cause → Implement (need explicit `n`)
CORRECT: User approves concept → Still need `n - next` for implementation

**Why:** Validates understanding; prevents wrong "obvious" fixes.

---

**BEFORE IMPLEMENTING (AI self-checks):**

- ☑️ D6.2: Presented fix options with pros/cons to user?
 → [User selected option OR only one option exists]
  
- ☑️ D6.3: IF modifying existing code → traced impact (dependents, dependencies, side effects)?
 → [Listed dependencies OR state "new code only, no modifications"]
  
- ☑️ User chose `n - next`?
 → [BLOCKING - cannot implement without this]

---

**IMPLEMENTATION (Choose path based on complexity):**

**SIMPLE FIXES (stay in DANEEL):**
- 1-3 components affected
- Logic corrections, state fixes, parameter adjustments
- No architectural changes
- Straightforward implementation

**Implementation approach:**
- Implement with proper architecture (apply CLIPPY.md C1/C2/C3 rules)
- Verify fix achieves user goal
- Present verification results

**COMPLEX FIXES (switch to CLIPPY.md A1):**
- Multi-component architectural changes (>5 components)
- New patterns or components needed
- Requires investigation of existing code patterns
- Needs incremental validation

**Transition to CLIPPY.md:**
```
Root cause identified → Present fix options
User: "n - next" for complex fix
AI: "This requires architectural changes. Please add @CLIPPY.md to context, then I'll proceed with A1 implementation protocol."
[User adds CLIPPY.md to chat]
📥 STEP 1: REQUEST - [Fix description]
🔍 STEP 2: INVESTIGATION & PROPOSAL
[Full A1 protocol follows]
```

**Important:** User must add `@CLIPPY.md` to chat context when switching protocols - AI needs access to full A1 rules for implementation.

**User can always request explicit switch:** "Use CLIPPY for this" forces full A1 protocol regardless of fix complexity.

**Why (match approach to complexity):** Simple fixes complete in protocol; complex fixes use full CLIPPY workflow.

---

**AFTER IMPLEMENTING (AI must verify):**

- ☑️ D6.6: Re-ran concrete example from A1.2 investigation?
 → [Show output now matches expected]
  
- ☑️ D6.6: Verified logical constraints satisfied?
 → [State which constraints now hold]
  
- ☑️ D6.6: Confirmed user goal achieved?
 → [State how fix enables user's goal from D2.2]

**Exit Protocol:** After fix verified with concrete example, logical constraints satisfied, user goal achieved.

---

**MENU ENFORCEMENT (A1.3):**

**MANDATORY:** Show menu at end of A1.3 phase and during discussion.

**Menu format:**
```
➡️ NEXT STEPS:

- n - next: Implement approved fix (only when user has selected fix approach)
- c - clarity: Verify root cause classification more deeply

... or provide free-form feedback/questions
```

**BLOCKING RULE (Binary Check):**

- [ ] Menu is LAST element in response?
  - NO → CANNOT proceed. Move menu to end before posting.
  - YES → Evidence: [Menu appears after all content, no operations after menu]

- [ ] Menu shown after free-form discussion?
  - NO → CANNOT proceed. Re-show menu after discussion response.
  - YES → Evidence: [Discussion response → menu re-shown]

**Free-form handling:** User can dispute root cause, question fix approach, or discuss alternatives. AI responds, maintains protocol state, re-shows menu, WAITS.

---

## D2: PURPOSE UNDERSTANDING

**Goal:** Establish what the system is trying to accomplish before investigating how it works.

**Referenced from:** A1.1 (UNDERSTAND PURPOSE phase)

**AI must determine:**

### D2.1: UNDERSTAND PURPOSE & CORRECTNESS

**AI must determine:**
- **Purpose:** What system does (operates on, produces, behaves)
- **User goal:** What user needs to accomplish
- **Correctness:** What makes behavior correct vs incorrect
- **Constraints:** Rules system must satisfy

### D2.4: INVESTIGATION TARGET SCOPING

**Investigation target types:**
- **My change:** Changed code + dependencies (Question: What assumption was wrong?)
- **The error:** Error message + context (Question: What concepts are involved?)
- **The system:** Architecture + domain concepts (Question: How is this designed?)

**Why:** Prevents investigating wrong layer.

### D2.5: SYMPTOM VS ROOT CAUSE DISTINCTION

**CRITICAL:** Explicit error does NOT mean root cause is understood.

**Before accepting error at face value, ask:**
- WHY does this code exist? What assumption led to this bug?
- Is this isolated or systemic? Are other places making same assumption?
- What's the correct pattern? How SHOULD this work architecturally?

**Example:**

❌ **BAD (fixing symptom):** "Component X calls unavailable operation Y" → Remove the call, done.

✅ **GOOD (understanding root cause):** Why does X need operation Y? What is X trying to accomplish? Find correct pattern. Check if other components make same assumption.

**Fix root causes, not symptoms.**

**Why:** Symptom fixes create debt; fix root causes.

### D2.6: ERROR CAUSATION VERIFICATION

**When error appears after your changes, verify causation:**

Avoid assumption: "I changed X, error appeared, therefore X caused it"

**Verify NEW vs PRE-EXISTING:**
1. Check history: When did this code last work?
2. Test isolation: If feasible, temporarily revert changes and test
3. Ask user: "Did this work before my changes?"

**If error is pre-existing:**
- Fix foundational issue first, then proceed with your work
- Building on broken foundation wastes effort

**If error is new:**
- Your change exposed or caused it
- Proceed with root cause investigation

**Regression signals:** Different symptoms indicate different root causes. Compare what changed between working state and broken state to locate where wrongness entered.

**Why:** Fix foundation before building.

### D2.7: UNFAMILIAR CONCEPT INVESTIGATION

**Trigger:** Error or code contains domain terms you don't understand.

**Before proceeding:** Understand domain concepts. Cannot debug what you don't understand.

**Concept investigation procedure:**

**1. Identify unfamiliar concepts:**
- Domain-specific terms in error messages
- Architectural abstractions in code
- Technical terms that aren't clear

**2. Investigate each concept:**
- Read component documentation
- Check domain model documentation
- Study examples showing concept usage

**3. Answer concept questions:**
- What IS this concept? (definition)
- WHY is it named this way? (purpose, distinction from alternatives)
- How does it RELATE to other concepts? (architecture)
- What are ALTERNATIVES? (e.g., "primary" vs "secondary", "cached" vs "fresh")

**4. Verify understanding:**
- Can you explain it simply?
- Do relationships make sense?
- Does error message make sense now?

**If concepts remain unclear:** Return to investigation. Complete concept understanding before proceeding to verification.

**Why:** Wrong model → wrong conclusions.

**OUTPUT:** Document understanding: INVESTIGATION TARGET | PURPOSE | USER GOAL | CORRECT BEHAVIOR | CONSTRAINTS | CONCEPTS CLARIFIED

**CRITICAL:** If you cannot articulate purpose clearly, STOP. Ask user to clarify before proceeding.

**Why:** Can't debug without knowing correct behavior.

---

## D3: GROUND TRUTH ESTABLISHMENT

**Goal:** Identify what's verified correct vs unknown vs incorrect, to narrow the problem space.

**Referenced from:** A1.2 (Investigation Types 1-2: Verify source state and components)

**AI must:**

### D3.1: IDENTIFY VERIFICATION SOURCES

**SOURCE HIERARCHY (AI must prioritize):**

Direct observation > Specs > Working code > History > Assumptions

- What external sources of truth exist?
  - Direct observation: Actual state (persistent storage, execution traces, system behavior)
  - Specifications, documentation
  - Reference implementations, working examples
  - User observations, domain knowledge
  - Assumptions (lowest priority - verify before relying on)

**Why:** Observation > specs > assumptions.

### D3.2: VERIFY COMPONENTS SYSTEMATICALLY

**PRINCIPLE:** Verify ENTIRE critical path before proceeding (not selectively).

**WHY complete path matters:**

**Reactive approach (incomplete):**
```
Execute → Error at step 1 → Fix → Execute → Error at step 2 → Fix → ...
Result: 5 iterations, discovering issues one at a time
```

**Proactive approach (complete path):**
```
Map ALL components in path → Verify each → Fix all issues → Execute → Success
Result: 1 iteration, all issues found via analysis
```

**Why:** Analysis finds all issues; execution finds one at a time.

**Investigation order (AI must follow):**

FIRST: Verify state at source
- Check state at source exists before investigating processing
- Count/sample actual state, mark [VERIFIED] or [INCORRECT]

THEN: Identify where behavior diverges
- Check state at each subsequent point
- Find FIRST point where state diverges from expected

NOT: Check result is wrong → investigate processing
CORRECT: Check source state exists → identify divergence point

### D3.2.1: VERIFY COMPONENT EXISTENCE & USAGE

**For each component in critical path:**
1. List all components (mark [ASSUMED])
2. Search codebase → Read implementation → Record contract → Mark [VERIFIED] or [NOT FOUND]
3. Find 3-5 usage examples → Extract pattern → Document initialization/usage/constraints
4. Expand scope as dependencies discovered

**Critical:** "Seems obvious" = hidden assumptions. Verify explicitly.

### D3.2.3: VERIFICATION STATUS TRACKING

For each system component, assign status:

- ✅ **[VERIFIED]** Component matches external truth source
 → [how verified - code read, examples found, behavior confirmed]
  - Do NOT re-check this component again
  
- ⚠️ **[UNVERIFIED]** Component not yet checked against truth
  - Needs verification before proceeding
  
- ❌ **[INCORRECT]** Component produces wrong results
 → [what's wrong, how discovered]
  - Potential root cause location

### D3.3: BUILD VERIFICATION MAP

**Format for documenting verification results:**

**VERIFICATION MAP FORMAT:**

✅ [VERIFIED] Component - Evidence + Location + Contract + Pattern
⚠️ [UNVERIFIED] Component - What needs checking
❌ [INCORRECT] Component - Problem + Evidence + Divergence  

**EXECUTION FLOW:** A → B → C → D with statuses

**IMPLICATIONS:** Problem NOT in [VERIFIED] → MUST be in [UNVERIFIED] between boundaries

**Why:** Emojis make state scannable.

**Completeness check before proceeding:**
- [ ] All components in critical path listed?
- [ ] Found working examples for EACH component?
- [ ] ZERO [ASSUMED] items remaining?
- [ ] Know how to reference each component?

**If any unchecked → verification incomplete → continue verification.**

**Why:** Map prevents circular investigation.

### D3.4: FACT RETENTION & HYPOTHESIS TRACKING

**Goal:** Build knowledge progressively without re-verifying proven facts, enumerate all hypotheses upfront, and systematically eliminate alternatives.

**AI must maintain THREE structures throughout debugging:**

**1. VERIFICATION MAP (Component Facts):**

**When fact is verified:**
1. **Immediately mark** component as [VERIFIED] in map
2. **Record evidence** used for verification
3. **Never re-check** this component again this session

**When proposing investigation:**
1. **Consult verification map** before each action
2. **Skip [VERIFIED] components** - they're foundation, not targets
3. **Focus on [UNVERIFIED]** components between verified boundaries

**When proposing fixes:**
1. **Never propose** changes to [VERIFIED] components
2. **Only target** [UNVERIFIED] or [INCORRECT] components
3. **Use [VERIFIED]** facts to narrow root cause location

**Exception:** Only re-verify if new evidence directly contradicts prior verification.

**2. HYPOTHESIS LIST (Root Cause Alternatives):**

**WHEN symptom identified, FIRST generate complete hypothesis list:**

**Hypothesis generation (mandatory before investigating mechanisms):**

BEFORE investigating specific theories:
- ☑️ Generated ALL plausible hypotheses?
 → [Listed 3-5 hypotheses explaining symptom]
- ☑️ Prioritized by evidence accessibility?
 → [Marked HIGH/MED/LOW based on testability]
- ☑️ Stated how to test each?
 → [For each hypothesis: "To verify: [action]"]

**Hypothesis statuses:**
- 💡 **[UNTESTED]** - Hypothesis identified, not yet investigated
  - Basis: [What observation suggests this]
  - To verify: [What would confirm/disprove]
  - Priority: [HIGH/MED/LOW - based on evidence accessibility]
- ✅ **[VERIFIED]** - Hypothesis confirmed with evidence
 → [What confirmed it]
- 🚫 **[ELIMINATED]** - Hypothesis disproven
 → [What disproved it]

**Priority levels (why prioritize):**
- **HIGH:** Direct observation of artifacts, quick to test
- **MED:** Code inspection required, moderate effort
- **LOW:** Requires extensive trace, theoretical

**Hypothesis list format:**

```
HYPOTHESIS LIST:

💡 [UNTESTED] H1: [Description] - Basis: [observation] | To verify: [test method] | Priority: [HIGH/MED/LOW]

✅ [VERIFIED] H2: [Description] - Evidence: [what confirmed it]

🚫 [ELIMINATED] H3: [Description] - Evidence: [what disproved it]
```

**When investigating:**
- Test HIGH priority first (quick evidence access)
- Update status after each test
- CANNOT conclude until all [ELIMINATED] except one [VERIFIED]

**BLOCKING:** Cannot proceed to A1.3 without systematic elimination showing all alternatives disproven except one.

NOT: Form hypothesis → Investigate it → Find supporting evidence → Conclude
NOT: "Might be X" → Continue investigating assuming X is true
NOT: Multiple hypotheses → Pick one and investigate without testing others

CORRECT: Generate complete list → Prioritize → Test highest priority → Mark [ELIMINATED] or [VERIFIED] → Test next → REPEAT
CORRECT: All hypotheses listed upfront → Systematic testing → Only conclude when alternatives eliminated
CORRECT: HIGH priority first → Direct observation before code inspection

**3. THEORY MAP (Investigation Theories):**

**When theory forms during investigation (AI writes "might be", "could be", "possibly"):**
1. **STOP investigation** immediately
2. **Add to hypothesis list** if not already present
3. **Mark as [UNTESTED]** in hypothesis list
4. **Present to user:** "Theory formed: [X]. Add to hypothesis list for testing?"
5. **Show menu**
6. **WAIT** for user direction

**When user requests theory verification:**
- Investigate ONLY that theory
- Return with result: [VERIFIED] or [ELIMINATED]
- Update hypothesis list
- Show menu
- WAIT for next direction

**BLOCKING:** Cannot continue investigation based on unverified theory. Must present theory to user first.

NOT: Form theory → Investigate based on theory → Form another theory
NOT: Multiple theories → Pick one and investigate without user input

CORRECT: Form theory → Add to hypothesis list → Present to user → Menu → WAIT
CORRECT: Theory formed → User chooses which to verify OR different path
CORRECT: One theory at a time, user-directed verification

**Combined Tracking Example:**

**VERIFICATION MAP (Components):**

✅ **[VERIFIED] Component A** - Matches external reference X (verified Step N)

✅ **[VERIFIED] Component B** - Result matches expected state Y (verified Step N+1)

⚠️ **[UNVERIFIED] Component C** - Not yet checked

⚠️ **[UNVERIFIED] Component D** - Not yet checked

❌ **[INCORRECT] Component E** - Produces wrong result Z (found Step N+3)
- Evidence: Expected X, observed Y
- Divergence: Only rows of type MONEYLINE fail, TOTAL rows succeed

**HYPOTHESIS LIST (Root Causes):**

💡 [UNTESTED] H1: Zero values in source data - Basis: Divergence shows failure pattern | To verify: Query source for zero values | Priority: HIGH

🚫 [ELIMINATED] H2: Wrong column structure in output - Evidence: Inspected artifact - TOTAL and MONEYLINE have same columns

💡 [UNTESTED] H3: Component C filters incorrectly - Basis: Component between verified B and incorrect E | To verify: Check filtering logic in Component C | Priority: MED

💡 [UNTESTED] H4: Component D transforms incorrectly - Basis: Component in execution path | To verify: Trace transformation in Component D | Priority: MED

**IMPLICATIONS:**
- Problem is NOT in A or B (verified correct)
- Problem MUST be in C, D, or source data (only unverified between verified boundaries)
- H2 eliminated (structure is consistent)
- Test H1 next (HIGH priority, quick to verify)

**NEXT:** Test H1 (inspect source data for zeros), then H3, then H4 if needed

**OUTPUT:** Verification map showing component status + hypothesis list showing alternative root causes with systematic elimination progress.

**Why:** Prevents early conclusion; ensures systematic elimination.

---

## D4: DOMAIN MODELING

**Goal:** Explicitly document how the system SHOULD behave according to logical rules, to identify semantic vs technical correctness.

**Referenced from:** A1.2 (Investigation Type 3: Build Domain Model), proposed by AI when constraints unclear

**AI must:**

### D4.1: DOCUMENT EXPECTED BEHAVIOR

**Template:** KEY ENTITIES (properties) | RELATIONSHIPS (constraints) | LOGICAL RULES (invariants)

### D4.2: CHECK LOGICAL CONSTRAINTS

Beyond checking individual values, verify logical constraints:

**Not just:** "Does this value exist and is it correct?"
**Also:** "Does this combination of values satisfy domain rules?"

**Common failure:** Individual components correct, but overall state violates logical constraints.

Example: Each probability value is valid (0.1, 0.5, 0.6) but sum > 100% (logically invalid)

### D4.3: IDENTIFY SEMANTIC VALIDITY ISSUES

**Check for impossible logical states:**
- Are entities grouped in ways that violate logical constraints?
- Do relationships exist that violate system invariants?
- Are operations using incompatible data?

**Examples - Semantic Invalidity:**

**Examples:**
- Resource after free: Technically works but semantically invalid (lifecycle violation)
- Incompatible units: Meters + seconds arithmetic works but meaningless (unit violation)  
- Dangling reference: Reference stored but target missing (integrity violation)

**OUTPUT:** Model showing expected behavior and semantic constraints.

**Why:** Finds semantic vs technical errors.

---

## D5: CONCRETE EXAMPLE TRACING

**Goal:** Follow ONE specific case from start to finish to find where expected behavior diverges from actual behavior.

**Referenced from:** A1.2 (Investigation Type 4: Trace Concrete Example), proposed by AI when concrete evidence needed

**AI must:**

### D5.1: SELECT CONCRETE CASE
- Pick ONE specific instance showing the problem
- Use actual identifiers, values, inputs (not abstractions like "value X" or "entity Y")
- Prefer simplest case that demonstrates issue
- State the case explicitly: "Tracing: [specific input/action/trigger]"

### D5.2: TRACE CASE THROUGH SYSTEM

**Trace progression:**
1. Identify system operations/decision points (don't assume architecture)
2. For each point: Document expected vs actual with concrete values
3. Mark MATCHES or DIVERGES at each point  
4. Stop at FIRST divergence - that's where wrongness enters

**Format:**
```
CONCRETE CASE: [Specific instance]
POINT 1: Expected [X] | Actual [Y] | Status: [MATCHES/DIVERGES]
POINT N: ...
DIVERGENCE: [First point where expected ≠ actual]
```

### D5.6: REGRESSION CHECK DURING TRACING

**TRIGGER:** When tracing after modification attempt.

**Compare:** BEFORE (state at points) vs AFTER (state at points) → Better/Same/Worse

**IF WORSE:**
- Check earlier points: Did modification break earlier component?
- Or: Did modification expose pre-existing issue in earlier component?
- Return to A1.2: Verify source state using P1 (source before result)

**Why:** Worse after fix signals earlier root cause.

---

## D6: ROOT CAUSE ANALYSIS

**Goal:** Determine WHY wrongness enters and implement correct fix.

**Referenced from:** A1.3 (ROOT CAUSE & FIX phase)

**AI must:**

### D6.1: UNDERSTAND ROOT CAUSE

Based on divergence point identified during A1.2 investigation, determine WHY the system produces wrong behavior:

**Key questions:**
- Is the code logic wrong?
- Is the data structure inadequate?
- Are components making incompatible assumptions?
- Is a required operation missing?
- Does code implement wrong conceptual model?

**Root causes:** Incorrect implementation | Invalid state | Component mismatch | Missing operation | Wrong model


### D6.2: EVALUATE FIX OPTIONS

For identified root cause, present options:

```
**ROOT CAUSE:** [Classification from D6.1]

**OPTION A:** [Approach 1]
  - Pros: [benefits]
  - Cons: [tradeoffs]
  - Scope: [how much to change]

**OPTION B:** [Approach 2]
  - Pros: [benefits]
  - Cons: [tradeoffs]
  - Scope: [how much to change]

**RECOMMENDATION:** [Which option and why]
```

### D6.3: IMPACT TRACE (for modifications to existing code)

**TRIGGER:** IF fix modifies existing code (not just adding new code).

**Before implementing modification, trace impact:**

**1. Dependents** (what depends on this):
- What code invokes this component?
- What data references this component's output?
- What constraints rely on current behavior?
- Search codebase for references

**2. Dependencies** (what this depends on):
- What must exist before this can work?
- What initialization is required?
- What state must be valid?

**3. Side effects** (what changes when you modify this):
- If changing identifiers → What references them?
- If changing contracts → What invocations will break?
- If changing persistence → What queries rely on old behavior?
- If changing timing → What race conditions might appear?

**Output:** Modifying [what] → Dependents [list] | Dependencies [list] | Side effects [list] → Assessment [SAFE/NEEDS_MIGRATION/REQUIRES_COORDINATION]

**Before proceeding:** Enumerate all dependencies. Unclear impact requires further investigation.

**Why:** Prevents unintended breakage.

### D6.4: PATTERN REPETITION AUDIT (for architectural bugs)

**TRIGGER:** IF root cause is architectural/systemic (not isolated typo/mistake).

**Decision criteria:**
- Skip if: Typo, syntax error, isolated one-off mistake
- MUST perform if:
  - Wrong component/pattern used (using X when should use Y)
  - Architectural assumption was wrong
  - Pattern could plausibly exist elsewhere
  - Error suggests design/pattern issue

**Procedure:**
1. Identify anti-pattern (what was used wrong?)
2. Search for all uses of same pattern
3. Classify each: SAME BUG vs different context (correct)
4. Expand fix to include ALL wrong instances

**Output format:**

Anti-pattern [description] → Instances [count] → Classification [WRONG/OK per instance] → Fix scope [all components]

**For architectural bugs:** Perform this audit. Skipping leaves repeated bugs creating technical debt.

**Why:** Architectural bugs repeat; fix all instances.

### D6.5: IMPLEMENT FIX

**Referenced from:** A1.3 (implementation section) - see A1.3 for complete execution control requirements

**Choose implementation path based on fix complexity:**

**SIMPLE FIXES (stay in DANEEL):**
- 1-3 components affected
- Logic corrections, state fixes, parameter adjustments
- No architectural changes
- Straightforward implementation

**COMPLEX FIXES (switch to CLIPPY.md A1):**
- Multi-component architectural changes (>5 components)
- New patterns or components needed
- Requires investigation of existing code patterns
- Needs incremental validation

**Why:** Match approach to fix complexity.

### D6.6: VERIFY LOGICAL CONSTRAINTS

After fix implemented:
- Confirm logical rules now satisfied
- Verify semantic validity restored
- Test with concrete example from A1.2 investigation
- Confirm user goal achieved

**OUTPUT:** Implemented fix with verification that logical constraints hold.

---

## PROTOCOL COMPLETION

**DANEEL complete when:** Root cause identified + fix approach selected

**Then:** Simple fixes → implement directly | Complex fixes → Switch to CLIPPY.md A1

**Final Verification:** Re-run concrete example | Verify constraints satisfied | Confirm user goal achieved

**After Successful Completion (AI behavior):**
- Communicate results in chat (not files)
- Do NOT create status docs/reports
- Task complete means share findings, not write documentation
- Only create documentation if user EXPLICITLY requests it

**Why:** No completion theater.

---

*Last Updated: November 25, 2025 (v2.13.0 - Inline enforcement refactor: Removed A1.4/A1.5/A1.6/A1.7/A1.8/A1.9/A1.10 extracted sections, inlined procedural enforcement into A1.1/A1.2/A1.3 phases where they execute. Follows CLIPPY.md v0.19.1 inline pattern and CONVENTIONS.md Section 4.5 guidance (procedural checklists inline, conceptual rules referenced). State machine preserved as brief A1 overview. D2-D6 conceptual sections unchanged. Result: 1488 → 1456 lines (32 removed, 2% reduction through consolidation). Previous: v2.12.0 menu simplification.)*

