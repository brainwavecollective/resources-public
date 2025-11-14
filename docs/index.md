---
title: Become a Member
sidebar_position: 0
pagination_next: null
pagination_prev: null
---

# Welcome to the Brain Wave Collective

The collective offers multiple membership paths to match your level of involvement. Each type of membership has distinct benefits and responsibilities.

## Find Your Path
```mermaid
%%{init: {'theme':'base', 'themeVariables': { 'fontSize':'18px'}}}%%
---
config:
  layout: dagre
---
flowchart TD
    START(["🚀 Join as a Community Member"]) --> Q0{"How would you like to engage?"}
    Q0 -- Strategic Partnership --> FIN@{ label: "💰 FINANCIAL SUPPORT<br>━━━━━━━━━━━━━━━<br>• Investment<br>• Philanthropy<br>• Direct Member Sponsorship<br>• In-Kind Resources" }
    Q0 -- Ideas & Skills --> SKILLS["💡 TALENT & EXPERTISE"]
    Q0 -- Not Sure Yet --> EXPLORE["🔍 Explore Options<br>with the Community"]
    FIN --> CINV(["✅ Member Type:<br><b>Contributing Investor</b>"])
    SKILLS --> Q1{"Do you have a specific<br>idea you are working on?"}
    Q1 --> YES_IDEA["Yes"] & NO_IDEA["No"]
    YES_IDEA --> Q2{"How do you want to develop your ideas?"}
    Q2 -- Solo Effort --> INDEP["🏃 Independently<br>Pursue your Project"]
    Q2 -- Collective Effort --> COLLAB["🤝 Collaborate &<br>Co-create"]
    INDEP --> CM
    COLLAB --> CIM(["✅ Member Type:<br><b>Active Contributor</b>"])
    NO_IDEA --> Q3{"Do you want to actively contribute to the Collective, and benefit from the contributions of others?"}
    Q3 --> SUPPORT_YES["Yes, contribute reciprocally"] & SUPPORT_NO["No"]
    SUPPORT_YES --> COLLAB
    SUPPORT_NO --> CM(["✅ Member Type:<br><b>Community Member</b>"])
    EXPLORE --> CM
    FIN@{ shape: rect}
    classDef startStyle fill:#43A047,stroke:#2E7D32,stroke-width:3px,color:#fff
    classDef questionStyle fill:#2196F3,stroke:#1565C0,stroke-width:2px,color:#fff
    classDef optionStyle fill:#E3F2FD,stroke:#1976D2,stroke-width:2px,color:#000
    classDef investorOutcome fill:#7B1FA2,stroke:#4A148C,stroke-width:3px,color:#fff
    classDef contributorOutcome fill:#FF6F00,stroke:#E65100,stroke-width:3px,color:#fff
    classDef communityOutcome fill:#43A047,stroke:#2E7D32,stroke-width:3px,color:#fff
    START:::startStyle
    Q0:::questionStyle
    FIN:::optionStyle
    SKILLS:::optionStyle
    CINV:::investorOutcome
    Q1:::questionStyle
    YES_IDEA:::optionStyle
    NO_IDEA:::optionStyle
    Q2:::questionStyle
    INDEP:::optionStyle
    COLLAB:::optionStyle
    CIM:::contributorOutcome
    Q3:::questionStyle
    SUPPORT_YES:::optionStyle
    SUPPORT_NO:::optionStyle
    EXPLORE:::optionStyle
    CM:::communityOutcome
```

*Not sure where to start? Explore more below...*

## Membership Types

### Community Member

**Basic Access**  

As a Community Member you form the foundation of the Brain Wave Collective. You can engage with the ecosystem while maintaining full independence. You may choose to limit your membership activities to community participation, temporarily be a community member while you learn about other opportunities, or level up and become a contributing member. In return for participating and supporting our mission, you gain access to:
- Designated events and community gatherings  
- Shared community resources  
- Information exchange and networking  
- Voting on select community matters

[Join the Brain Wave Collective](https://brainwavecollective.ai/resources/join/)  
→ Access events, resources, and become part of our growing community.

---

### Contributing Member - Talent

**Core Participation**  

As a member directly contributing your talent you become an invention catalyst who helps drive collective value creation. Whether you're a builder, inventor, or expert contributor, your sustained efforts create value for the collective. In return, you gain deeper access to Cooperative resources and expanded rights defined by contributor agreements, including:
- All Community Member rights  
- Access to collective resources  
- Participation in collaborative projects  
- Expanded voting power  
- Enhanced patronage allocation  

[Join the Brain Wave Collective](https://brainwavecollective.ai/resources/join/)  
→ Indicate interest in contributing talent and expertise during sign-up.

---

### Contributing Member - Capital

**Financial Enablement**  

As a strategic partner who helps support the Cooperative's long-term growth through various forms of funding and collaboration. Whether through investment, grants, or partnerships, your support drives breakthrough innovation. In return, you gain access to financial returns, opportunities, and structured governance participation as outlined in contributing agreements. This includes:
- All Community Member rights  
- Access to financial return mechanisms as defined in agreements  
- Participation in Cooperative governance  
- Access to select financial and operational information

[Join the Brain Wave Collective](https://brainwavecollective.ai/resources/join/)  
→ Indicate interest in capital contribution during sign-up.

---

## Membership Principles

Our membership framework reflects our values: collaboration, shared resources, democratic control, and innovation acceleration. We’re building an ecosystem for the future—where your independence and our collective progress go hand in hand.

[Read our Code of Conduct](./Documentation/code_of_conduct) to learn more about our community standards.

*All members are at a minimum Community Members. You may elect to join one or more additional classes (e.g., Contributor or Investor), each offering unique commitments, benefits, and responsibilities.*

**Ready to build with us?**  

**[Join the Brain Wave Collective.](https://brainwavecollective.ai/resources/join/)**
