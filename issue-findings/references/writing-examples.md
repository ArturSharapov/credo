# Writing examples

The report below is fictional. Its code, checks, and results illustrate writing choices, not an investigation performed for a real project. It omits file links because the files do not exist. Actual reports link verified sources and affected files as required by the template.

## Example report

```markdown
# Solutions for configuration and import failures

The example covers a timeout reader and a record importer. **The changes below are recommendations, not implemented features.**

---

## 1. A zero timeout enables the default timeout

Setting `timeoutMs` to `0` should disable the timer. The reader uses `options.timeoutMs || 5000`, so it replaces zero with five seconds. A caller that explicitly disabled the timer can therefore receive a timeout error.

A check of the reader returned 5000 for both zero and an omitted value. A prototype used an explicit undefined check and returned zero, the configured positive value, and the default for those respective inputs. The existing validator already rejects null and negative values. Changing validation would not fix the reader's treatment of a valid zero. These checks exercised the reader; an actual timed operation remains untested.

---

**Before:**
The timeout reader replaces an explicit zero with five seconds, enabling a timer the caller disabled.

**After:**
Zero disables the timer. An omitted timeout still uses five seconds, and positive values retain their configured duration. Null and negative values remain invalid.

**Best solution:**
In `readTimeout`, return 5000 only when `options.timeoutMs` is undefined. Otherwise return the supplied value. Keep the existing validator and timer interface unchanged.

---

## 2. A failed import leaves some records saved

Importing ten records can save the first six before the seventh fails validation. The caller receives an error, but retrying the corrected file encounters records from the failed attempt. `importRecords` validates and saves one record at a time without a transaction.

Validating the entire input before saving prevented partial writes on validation errors in a prototype. It required retaining the complete input and still left earlier writes saved when a later database operation failed. A second prototype wrapped the existing loop in a database transaction. Injected validation and write failures rolled back earlier writes; a successful import saved every record.

The transaction check used a local database. It did not establish behavior after a connection loss or whether holding a transaction for a large import causes unacceptable contention. The importer accepts database records only; the check does not establish rollback for external actions such as sending email.

---

**Before:**
An import error can leave earlier records saved. The caller cannot safely retry the complete corrected input without first identifying those records.

**After:**
A successful import saves every record. A validation or write failure before commit leaves none of that import's records saved. Connection loss during commit still requires a separate check before promising a safe automatic retry.

**Best solution:**
Have `importRecords` own one transaction for the complete import:

1. Open the transaction before reading and validating the first record.
2. Pass its connection to the existing record writer for every insert. Keep validation inside the loop so records can still be read incrementally.
3. Commit only after the input has ended and every write has succeeded.
4. On failure, roll back and release the connection. Preserve the original validation or write error if cleanup also fails.

Keep the existing record format and validation rules. Do not split the input into independently committed batches, because that would retain partial imports. Confirm connection-loss behavior and contention at supported import sizes before accepting this recommendation for production.

## Verification and sources

The checks described above used isolated prototypes. Production code is unchanged. Issue-specific results and remaining checks appear above.

The example's source material consists of the timeout reader, its validator, the record importer, and the local reproduction notes. In an actual report, link the corresponding files and checks.
```

## What to transfer

- **A concrete opening.** The zero-timeout example names the input, the unexpected result, and its consequence before proposing a fix. The import example uses a failing seventh record to explain why partial writes matter.
- **Comparisons with consequences.** Prevalidation handles one failure but leaves another exposed and retains the full input. Those facts explain the recommendation more precisely than calling it simpler or more robust.
- **Bounded evidence.** Reader checks do not establish timer behavior. A local rollback check does not establish recovery after connection loss. Put each limitation beside the result it qualifies.
- **Useful restatement.** Before briefly restates the failure without repeating the experiments. After states the intended behavior and its limits. Best solution supplies the changes needed to produce it.
- **Proportional detail.** The reader fix needs one paragraph. The import needs ordered steps and compatibility constraints. Use paragraphs for independent requirements and lists when sequence matters; choose the length from the work, not the example.

Preserve these relationships when the subject changes. Keep technical terms only when they help explain the current issue, and retain every condition that changes the behavior or recommendation.
