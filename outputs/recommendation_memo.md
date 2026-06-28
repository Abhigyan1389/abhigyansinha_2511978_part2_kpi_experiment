**BUSINESS ANALYST PROBLEM STATEMENT**
Campaign Experiment — KPI Evaluation & Scale Decision
CONTEXT- 	A controlled A/B experiment was run across 1,408 users (693 Control, 715 Treatment) spanning 4 regions, 3 device types, 5 traffic channels, and 3 plan tiers. The Treatment variant produced materially higher conversion but also introduced new operational signals that require scrutiny before a scale decision is made.
1. The Decision That Needs to Be Made
Should the company permanently replace the current user acquisition and onboarding experience (Control) with the newly tested campaign variant (Treatment), and invest in scaling it across all regions, devices, and traffic channels?
This is a go/no-go decision on committing:
•	Marketing budget and campaign channel strategy
•	Product engineering resources for full onboarding flow implementation
•	GTM strategy aligned to the Treatment variant's funnel structure
A partial rollout (e.g., specific regions or traffic sources only) remains on the table if sub-group results are inconsistent.
2. Who the Decision Impacts
Stakeholder	Impact
Growth & Marketing-	Budget allocation, landing page design, and channel strategy will shift based on the outcome
Product Team-	Onboarding flow changes may require engineering work to operationalize at scale
Revenue & Finance-	Conversion rate and 30-day revenue per user drive CAC payback periods and LTV forecasts
Customer Support-	Treatment already shows 70% higher ticket rate (0.37 vs 0.22) — direct staffing cost implications
New Users	- All future users will be funnelled through whichever experience is selected
Free Plan Users	- argest segment (729 of 1,408 users) and the primary future upsell target post-decision
3. What Metrics Should Improve
Primary KPI
PRIMARY	- Trial-to-Paid Conversion Rate — Treatment: 7.0% vs. Control: 3.2% (~120% relative uplift)
Supporting KPIs — All Must Move in the Right Direction
Metric	                                       Control	                      Treatment
Completed Onboarding Rate	                     15.6%	                        21.3% ✅
Avg. Revenue in 30 Days (per user)	           $51.75	                        $53.88 ✅
Days to Convert	                               8.9 days	                      6.4 days ✅
Engagement Score	                             57.0	                          62.9 ✅
Landing Page Visit Rate	                       63.6%	                        72.6% ✅
Support Tickets (per user)	                   0.22	                          0.37 ⚠️
Refund Rate	                                   0.00%	                        0.42% ⚠️
The Treatment drives faster decisions, stronger engagement, and better onboarding completion. However, the support and refund signals warrant investigation before treating the conversion lift as the full picture.
4. Risks That Must Be Monitored
Risk	                                              Description	                                                 Signal in Data
Support Burden	                                  Treatment users generate 70% more support tickets per user.   0.37 vs 0.22 tickets/user
                                                   If driven by onboarding confusion or unmet expectations,
                                                  scaling without fixing this will erode revenue gains through
                                                  operational costs.	
Refund Rate Emergence	                            Control shows zero refunds. Treatment already has a 0.42%      0.42% vs 0.00%
                                                  rate — a directional flag that the campaign may be attracting
                                                  misaligned users or overpromising on product value.	
Revenue Concentration	                            Average revenue figures can be misleading if a small number    Wide revenue range in Treatment converters
                                                  of high-value converters are inflating the Treatment mean.
                                                  LTV distribution needs outlier analysis.	
Segment Inconsistency	                           Aggregate results may mask underperformance in specific         4 regions, 3 devices, 5 traffic sources
                                                 sub-groups.Premium plan users or Tablet device users may respond
                                                 differently to the Treatment variant.	
5. Evidence Required Before Making a Recommendation
   
Statistical Validity
•	Confirm the conversion rate difference (7.0% vs 3.2%) is statistically significant at p < 0.05 using a chi-square or z-test for proportions
•	Verify no significant pre-experiment imbalance exists between Control and Treatment (randomisation check)

Economic Viability
•	Calculate revenue per user across the full cohort — not just among converters — to account for funnel-wide cost
•	Model the support cost impact of a 70% higher ticket rate at scale to confirm net margin improvement holds

Segment-Level Breakdown
•	Run conversion analysis cut by region, device type, traffic source, and plan type
•	Confirm the Treatment effect is consistent across segments — not driven by a single high-performing sub-group
•	Confirm Treatment does not significantly underperform Control in any major segment

Durability Check
•	Confirm the experiment ran long enough to eliminate novelty effect bias
•	Verify conversion rates stabilised toward the end of the observation window rather than still trending

Quality Signal Resolution
•	Investigate the nature of refund requests in the Treatment group — identify if tied to a specific channel or plan type
•	If elevated support tickets are driven by a specific onboarding step, confirm that step can be fixed before full rollout
DECISION GATE	- Statistical significance confirmed  +  Support cost modelled at scale  +  Segment consistency verified across all major sub-groups. All three conditions must be met before a full-scale rollout recommendation is issued.
**Recommendation Memo**
**Executive Summary**
VERDICT- CONDITIONAL LAUNCH — Conversion, onboarding and engagement are all statistically confirmed improvements. Full rollout is blocked by a support ticket 
spike that cancels the net revenue advantage. Launch immediately to Premium  plan + Paid Search users where economics are clearly positive.
The experiment ran across 1,400 deduplicated users (Control: 690, Treatment: 710). Treatment doubled paid conversion rates (3.19% → 7.04%, p=0.001), reduced days-to-convert from 8.9 to 6.4 days, and improved engagement scores by +10.4% (p<0.001) across all tested segments.However, Treatment users raised support tickets at 67.7% higher rates (14.8% → 24.8%, p<0.001). At $15 per ticket, this adds $2.29/user in operational cost — exactly cancelling the $2.28/user gross RPAU gain. Net RPAU after support cost: Control $48.67 vs Treatment $48.66. A full rollout at current support levels would scale cost as fast as revenue.The recommended path is an immediate targeted launch to Premium plan + Paid Search — the two cohorts where Treatment net RPAU is positive, support ticket lift is lowest (+12–18%), and engagement improves most (+14.9–15.7%). This captures early revenue while the product team resolves the onboarding friction generating excess support contacts.
**North Star Metric**
NSM- Revenue Per Acquired User (RPAU) — 30 Days  =  Total 30-day Revenue ÷ Total Users in Group
RPAU was selected because it integrates conversion volume and revenue quality in a single number. Conversion rate ignores deal size; average revenue per converter is distorted by outliers and ignores the 93% of users who never pay. RPAU captures both and is the most honest basis for a scale decision
RPAU result: Control $51.97 vs Treatment $54.25, +4.4% lift, Mann-Whitney U p=0.001. This is the primary evidence of revenue improvement. The critical caveat: this gross advantage is cancelled once support costs are netted (see Section 6).
**KPI Tree Explanation**
The KPI tree maps each experiment metric to its role in driving RPAU:
<img width="489" height="304" alt="image" src="https://github.com/user-attachments/assets/00b1cbd2-d531-463a-98c0-5d2cf0e459f3" />
Note on Driver 2: the $1,630 vs $770 gap is not statistically significant (p=0.064) and is driven by 3  Control outliers with revenues of $8,611, $6,789 and $3,888. Removing them drops Control average to ~$640 — below Treatment. The RPAU level (which includes all 1,400 users) is the more reliable comparison.
**Experiment Result Summary**
<img width="485" height="280" alt="image" src="https://github.com/user-attachments/assets/79758d42-71f4-450a-b532-66c5fed264b8" />
The experiment was well-powered (observed power 90.4%, above the 80% requirement). Sample sizes of 690 and 710 are sufficient to detect the effects observed. The result pattern is unambiguous: Treatment wins on every acquisition and engagement metric; the only confirmed negative is the support ticket breach.
**Hyothesis Test Interpretation**
<img width="545" height="154" alt="image" src="https://github.com/user-attachments/assets/1f4e988a-9861-4eea-951f-a8083cfc5a70" />
The z-statistic of 3.264 exceeds ±1.96. The p-value of 0.0011 is well below α=0.05. The 95% CI for the difference [+1.54pp, +6.17pp] excludes zero — a second independent confirmation. Absolute lift: +3.85pp. Relative lift: +120.9%. This is decisive.
<img width="520" height="129" alt="image" src="https://github.com/user-attachments/assets/9cb05064-19a8-4362-9554-8fec7dcf0bad" />
RPAU is significantly higher in Treatment ($54.25 vs $51.97, p=0.0013). Mann-Whitney U was used because 93% of users have $0 revenue and outliers create extreme right-skew, violating t-test normality assumptions. The gross margin of +$2.28/user is real — but cancelled by +$2.29/user in additional support cost (see Section 6).
<img width="489" height="184" alt="image" src="https://github.com/user-attachments/assets/cab5e9ba-1b64-489a-b40f-6654955d8944" />
z=4.692 implies the probability of this result under H₀ is less than 0.003%. The +67.7% increase (14.8% → 24.8%) is confirmed across every region, plan, device, and traffic channel. No segment is exempt. This result directly blocks full-scale rollout.
**Guardrail Analysis**
<img width="533" height="77" alt="image" src="https://github.com/user-attachments/assets/305453c2-5e73-414a-a293-f24af4a7aba3" />
Support Ticket Rate — confirmed blocker
The breach is universal — worst in Referral (+131%), Email (+106%), Free plan (+97%), Tab (+96%); mildest in Paid Search (+18%) and Premium (+12%). The spike lives almost entirely in first time contacts (users with 1 ticket: 9.3% → 16.1%), pointing to a single onboarding friction step rather than a general product quality decline.Financial consequence: Control support cost = $3.30/user (0.220 tickets × $15). Treatment support cost = $5.60/user (0.373 tickets × $15). Difference = +$2.29/user. This directly offsets the gross RPAU gain of +$2.28/user. Net RPAU: $48.67 vs $48.66.
Refund Rate — emerging concern
Fisher Exact p=0.250 — not significant at n=3 events. However: all 3 refunds are from mobile users, 2 of 3 are in the West region, and the converter refund rate is 6.0% (3/50) in Treatment vs 0.0% (0/22) in Control. Treatment is 0.08pp below the 0.50% cap. At 10× scale, a 6% converter refund rate produces ~30 events — highly significant.
Engagement Score — passed and a launch asset
t=7.93, p<0.0001 — the most statistically certain result in the experiment. Holds across 15 of 16 segments. The Very High band (score 80+) more than doubled from 5.6% to 12.4% of users. Engagement is independent of support tickets — high-engagement Treatment users still raise tickets at 23.2% vs 12.8% for equivalently engaged Control users. These require separate fixes.
**Segment Level Insights**
<img width="511" height="286" alt="image" src="https://github.com/user-attachments/assets/787bc6b6-8fc5-4992-a2eb-eb7a2b16b6a8" />
Why Premium Plan + Paid Search
Premium plan Treatment users show the best overall economics: +127% conversion, +86.7% RPAU ($100.56 vs $53.86), only +12.3% support ticket lift, and +15.7% engagement — the largest of any segment. Paid Search adds +384% conversion, +219% RPAU, and only +18.4% support lift — the second-lowest in the data. Both cohorts appear more deliberate and higher-intent, which likely explains why the Treatment experience creates less confusion for them.Free plan, Referral, Email, and Tablet are the highest-priority segments for the follow-up experiment. They show the largest conversion lifts in the data but also the largest support ticket spikes. They represent significant revenue upside — but not until the onboarding friction is resolved.
**Final Recommendation**
RECOMMENDATION
CONDITIONAL LAUNCH — Premium plan + Paid Search segment only
Do not launch to full audience. Fix support ticket issue. Retest before broad rollout.

The case for launching now — selected segment only
Premium + Paid Search users represent a cohort where the Treatment already works on all three dimensions: net economics are positive, support risk is contained, and engagement is the highest in the experiment. Delaying any launch means forgoing confirmed revenue improvement in a segment where 
the risk profile is already acceptable.
Launch conditions: route only Premium plan and Paid Search traffic to Treatment. Monitor support ticket rate weekly; pause if it approaches 20%. Monitor refund rate daily; pause if it reaches 0.45%. Collect 30→60 day retention data to validate the LTV thesis.
The case against full rollout
At 10× scale (14,000 users), gross revenue advantage ≈ +$31,900. Additional support cost at 0.373 tickets/user vs 0.220 × $15 × 14,000 users ≈ +$32,100. Net result ≈ −$197. A full rollout would cost more to operate than it earns in incremental revenue.
Additionally, all 3 Treatment refunds are from mobile users. If mobile traffic is a large share of the scaled audience, the refund rate could breach the 0.50% cap before the team can respond — creating billing reversal and reputational risk
**Risks and Limitations**
<img width="455" height="376" alt="image" src="https://github.com/user-attachments/assets/f707cbdc-b881-42cc-8447-3f85e5b57831" />
**Next Steps**
Immediate — within 2 weeks
• Launch Treatment to Premium plan + Paid Search traffic only — implement routing logic in acquisition and onboarding pipeline
• Audit 3 refund records (mobile, West region) — identify the specific step triggering refund requests
• Audit 3 Control revenue outliers — verify billing amounts are accurate and not pipeline errors
• Confirm actual support cost per ticket with operations team — refine net RPAU with real cost data
Near-term — weeks 2 to 6
• Map the complete Treatment onboarding journey step by step — identify which step correlates with 1 ticket contacts
• Design and run a micro-experiment fixing the identified friction step — target ticket rate below 14.8% (Control baseline)
• Collect 30→60 day retention data for the launched cohort — use as primary KPI for the next go/no-go gate
• Set monitoring alerts: pause Treatment if support ticket rate exceeds 20% in launched segment; pause if refund rate exceeds 0.45%
<img width="464" height="156" alt="image" src="https://github.com/user-attachments/assets/a4712826-21a4-4657-b544-10c97a1abe5a" />

