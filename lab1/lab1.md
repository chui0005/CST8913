# [Nourish + Bloom Market – Azure Cloud Migration](https://www.microsoft.com/en/customers/story/1543855863468416507-nourishandbloom-market-azure#section-block-body)

## 1. What are the company's motivators of migration to the cloud?
- **Seamless, touchless shopping experience**  
  They want to leverage IOT and the cloud to enable customers to simply walk out of the store after picking items—making the experience fast and frictionless.

- **Scalability for future growth**  
  Since they have plans for rapid expansion—including flexible, shipping-container-based pop-ups. They need the technology to be able to scale with them and be asscessible in multiple regions.

---

## 2. What questions will you ask to understand more about the company infrastructure?
1. What kind of backend systems run the checkout, inventory, and sensor data?  
2. Are there on-premises components, or is everything already in Azure?  
3. How are cameras, sensors, and mobile apps currently managed, and what volumes of data do they generate?  
4. What are the security, compliance, and privacy policies governing sensor and customer data?  
5. Are there regional deployment needs, especially when scaling to new locations?  
6. What level of technical expertise and resources does the team have for managing Azure services?  

---

## 3. RACI Matrix – Migration Stakeholders

| Task / Role                                | Store Owners / Founders | Azure Engineer / IT Team | UST Partner | Store Staff |
|--------------------------------------------|--------------------------|---------------------------|-------------|-------------|
| Define business objectives                  | R                        | C                         | C           | C           |
| Inventory of existing systems               | R                        | A                         | C           | I           |
| Evaluate Azure services & architecture      | I                        | A                         | R           | I           |
| Design migration plan                       | I                        | A                         | R           | I           |
| Execute migration (deploy backend systems)  | I                        | A                         | R           | I           |
| Testing & validation                        | I                        | R                         | C           | I           |
| Staff training & onboarding                 | R                        | C                         | C           | A           |
| Go-live and ongoing support                 | R                        | A                         | C           | R           |


**Legend:**  
- **R** = Responsible (does the work)  
- **A** = Accountable (owns the result)  
- **C** = Consulted (provides input)  
- **I** = Informed (updated on progress)  

---

## 4. Describe the most likely migration approach that will suit the chosen company.
From the article, it seems the first Nourish + Bloom store was not originally a fully cashless, walk-out experience. Because of this, a Refactor migration approach would be most suitable.
- Keep the few existing systems that are still relevant and can be migrated into Azure.
- Leverage PaaS services (like managed databases, IoT Hub, and security tools) to modernize the rest of the environment.

This approach allows the company to re-architect around Azure capabilities, rather than just lifting older systems that don’t meet the seamless, touchless shopping vision.

---

## 5. Produce a high level schedule for the migration process.

Phase 1 – Weeks 1–2: Planning & Assessment
- Gather current infrastructure details
- Set goals, define scope, align on architecture

Phase 2 – Weeks 3–5: Design & Preparation
- Design Azure solution architecture
- Prepare Azure environment and access
- Define testing scenarios and metrics

Phase 3 – Weeks 6–8: Pilot Migration
- Migrate one store’s backend systems
- Configure sensors, inventory, checkout flows
- Test functionality, security, and performance

Phase 4 – Weeks 9–10: Training & Validation
- Train store staff on new processes
- Conduct usability testing with customers and staff

Phase 5 – Ongoing: Operations & Optimization
- Monitor systems, security, and SLAs
- Continuously improve performance and scale

## 6. What are the main decision criteria for the chosen company?
1) Customer experience: The technology need to be easy to use and orperate behind the scenes.
2) Security: Customer must trust their information is stored safely.
3) Scalability: Technology needs to scale not just with traffic in one store but handle new locations being added.
