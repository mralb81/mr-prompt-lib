# Query utili per la dashboard

## Utenze attive nel mese

```sql
select count(distinct(assignee_login)) from copilot.billing_daily
where report_date = '2026-03-01' and pending_cancellation_date is null
and lower(assigning_team_group_name) <> 'github_italy_generali_invest'
and lower(assigning_team_group_name ) <> 'github_italy_bancagenerali'
-- Sempre inizio del mese successivo
```

## LOC agentiche accettate per mese (in questo caso marzo)

```sql
select sum (du.loc_added_sum) / 20 / 1039 from copilot.dashboard_users du
where du."day" between '2026-03-04' and '2026-03-31'
and du.used_agent;
-- Ultimi 28 giorni, togliere inv. holding e bancagenerali
```
