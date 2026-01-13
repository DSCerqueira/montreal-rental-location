# Montreal Rental Location Decision Platform – Business Case

## Overview
This business case presents a data-driven decision-support platform designed to help renters identify the most suitable borough on the Island of Montreal based on their individual needs. By consolidating fragmented public and third-party data into a single dashboard, the solution reduces uncertainty and improves confidence in rental decisions.

## Problem Statement
Renters face significant difficulty comparing Montreal boroughs due to:
- Dispersed and inconsistent information sources
- Limited ability to evaluate trade-offs (price vs. commute, safety vs. services)
- Location issues often discovered only after moving in

There is currently no unified tool that evaluates rental locations holistically and according to user-defined priorities.

## Business Goals
- Simplify rental location decision-making
- Enable objective, standardized comparison across boroughs
- Allow users to prioritize criteria based on personal needs
- Identify boroughs offering the best balance of affordability, safety, accessibility, and quality of life
- Support landlords and real estate stakeholders in estimating competitive rental prices

### Success Criteria
- Clear and intuitive user experience
- Reliable and transparent data
- Actionable insights that support real-world decisions

## Solution Summary
The proposed solution is an interactive Power BI dashboard that evaluates rental locations using eight core criteria:
- Average rental price
- Safety (infraction ratio)
- Commute time to downtown
- Walkability
- Public transportation quality
- Daycare availability
- School availability
- Secondary school quality

Users assign weights to each criterion (from zero to high importance). Based on these priorities, the dashboard ranks locations and recommends the top three boroughs, clearly explaining the rationale behind each recommendation.

## Methodology
- Rank boroughs independently for each criterion
- Apply user-defined weights to calculate a composite score
- Normalize scores to ensure transparency and fairness
- Present results through ranked recommendations and visual explanations

## Risks and Mitigation
- **Data Quality:** validation, cleaning, and standardization
- **Platform Limits:** optimized models and scoped features
- **Resource Constraints:** phased delivery and focused scope
- **User Experience:** guidance, tooltips, and examples
- **Analytical Bias:** transparent logic and scenario testing

## Implementation Roadmap
### Phase 1 – Planning
- Define requirements, KPIs, and scope
- Identify data sources
- Design architecture and visualization strategy

### Phase 2 – Development
- Data ingestion and transformation
- Semantic model and KPI development
- Dashboard creation and refresh scheduling
- Documentation and pipelines

### Phase 3 – Integration
- KPI validation
- GitHub and DevOps integration

## Continuous Improvement
Future enhancements include income, demographics, language, healthcare access, and machine learning models to estimate rental prices based on contributing factors.

## Conclusion
This solution transforms fragmented urban data into a unified, user-centric platform that reduces decision risk and delivers value to renters and property stakeholders.
