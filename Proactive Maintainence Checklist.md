# UpFront

- **Enable Scheduled Activities**  
  - [GitHub Link: Web Reports Scheduled Activities](https://github.com/mpaishon/WebReportsSchedActivity)

- **SQL Best Practices**  
  - [GitHub Link: SQL Best Practices for BigFix](https://github.com/mpaishon/BigFixDocs/blob/main/Bfe%20Sql%20Best%20Practices.md)

- **Establish Good Usage Practices**  
  - [GitHub Link: BigFix Best and Bad Practices](https://github.com/mpaishon/BigFixDocs/blob/main/Best%20and%20Bad%20Practices%20BigFix.md)

- **Understand Your Client Loop and why it matters**  
  - [GitHub Link: Client Loop WP](https://github.com/mpaishon/BigFixDocs/blob/main/Elegance%20of%20the%20BF%20Client%20Loop.md)
---

## Daily Tasks

- **[SEC] - Verify Source of Prefetch/Downloads for Custom Content**  
  - [GitHub Link: Query Sites in Prefetch for Whitelist](https://github.com/mpaishon/HelpfulSessionRelQueries/blob/main/Query%20Sites%20in%20Prefetch%20for%20Whitelist.md)

- **Verify Core Root Services**
  - Verify `FillDB`, `Root`, and `GatherDB` completion.
- **Verification of Completion for SQL Maintenance Jobs**


---

## Weekly Tasks

- **Check Large Impact Properties**  
  - [GitHub Link: Largest Properties Query](https://github.com/mpaishon/BFSqlEntQueries/blob/main/BFE%20Largest%20Properties.md)

- **Verify ActionSite Size**  
  - Preferred size under 10MB.

- **Run Audit Trail Cleaner**  
  - Frequency may vary based on environment needs.

- **Run BES Computer Remover**  
  - Frequency may vary based on environment needs.

- **Check License Dashboard within Console**

- **Delete Stopped or Expired Actions**  
  - Automate with Platform REST API if possible.

- **Run a Custom Blank Action**  
  - Verifies communications and notification of ActionSite.

- **Check Action Site Versions on Relays**  
  - [GitHub Link: ActionSite Version on Relays Query](https://github.com/mpaishon/HelpfulSessionRelQueries/blob/main/Actionsite%20version%20on%20relays.md)

- **Evaluate Weekly**

---

## Event-Driven Tasks

- **Monitor Problematic Action Content**  
  - [GitHub Link: Scheduled Report - Notify Client ForceRefresh](https://github.com/mpaishon/WebReportsSchedActivity/blob/main/_Scheduled%20Report-Notify%20ClientForceRefresh.txt)
	Adapt using example as necessary.
	
- **Monitor Problematic Client Relevance**  
  - Adapt using action example as necessary.

- **Monitor for New OS in the Environment**  
  - May need to add Patch Sites or SCM Content.
  - [GitHub Link: Scheduled Report - Unique Operating Systems](https://github.com/mpaishon/WebReportsSchedActivity/blob/main/_Scheduled%20Report-Unique%20Operating%20Systems%20.txt)

- **Monitor New Manual Cache Requirements**  
  - [GitHub Link: Scheduled Report - Manual Cache Required (Actions)](https://github.com/mpaishon/WebReportsSchedActivity/blob/main/_Scheduled%20Report-Manual%20Cache%20Required%20(Actions).txt)

- **Monitor Client Settings of Interest**  
  - [GitHub Link: Query Actions Affecting Work Sleep Idle Settings](https://github.com/mpaishon/HelpfulSessionRelQueries/blob/main/Query%20Actions%20Affecting%20Work%20Sleep%20Idle%20Settings.md)
