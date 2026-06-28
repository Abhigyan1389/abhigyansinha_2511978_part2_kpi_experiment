BUSINESS ANALYST PROBLEM STATEMENT
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
