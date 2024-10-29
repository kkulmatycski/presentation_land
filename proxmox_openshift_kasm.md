# Modernizing Our Virtualization Infrastructure
## Strategizinig our Infrastructure Deployements and Administration

### Automated Infrastructure and Configuration Management
[10/29/2024]
[Kyle Kulmatycski]

---

## Current State and Drivers

### Current Environment
* Test: VMware ESXi
* Production: RHEVM (EOL -- Extended life support until 2026)
* Remote Access: NoMachine NX
* Manual Provisioning
* Limited Configuration Management

### Driving Change
* RHEVM end-of-life
* Need for automated provisioning
* Remote access modernization (video encoding / container isolation per user)
* Configuration management automation (try to centralize)

---

## Proposed Architecture

### Modern Automated Stack

#### 1. Infrastructure Layer
* Proxmox VE replacing VMware
* Feature parity with current stack
* API-driven automation
* 80% lower licensing costs

#### 2. Container Platform
* OpenShift replacing RHEVM
* Modern orchestration
* Natural RHEL progression
* Future-proof architecture

#### 3. Remote Access
* Kasm replacing NoMachine NX
* Browser-based solution
* Enhanced security
* Improved user experience

#### 4. Automation Layer
* Ansible integration throughout
* Infrastructure as Code
* Configuration Management
* Automated deployments

---

## Ansible Integration Strategy

### Infrastructure Provisioning
* Proxmox VM deployment
* OpenShift cluster management
* Network/storage configuration
* Template-based deployment

### Configuration Management
* System hardening
* Package management
* Service configuration
* Compliance enforcement

### Integration Points
* Proxmox API automation
* OpenShift operator integration
* Git-based workflow
* CI/CD pipeline integration

---

## Platform Deep Dive

### Feature Comparison

| Feature | Current | Proposed |
|---------|---------|----------|
| Virtualization | VMware/RHEVM | Proxmox |
| Container Support | Limited | Native |
| Remote Access | Client-based | Browser-based |
| Automation | Manual | API-driven |
| Deployment Time | Minutes | Hour(s) |
| Configuration | Automation through scripts | Automation through API |

### Benefits
* Consistent deployments
* Reduced manual effort
* Enhanced security
* Improved scalability
* Modern architecture

---

## Implementation Approach

### Phase 1 (4-6 months)
* Proxmox deployment 
* Ansible framework setup
* Initial automation development
* Test environment migration

### Phase 2 (3-4 Months)
* OpenShift implementation (not including migrating existing services)
* RHEVM workload migration
* Automation integration (migrate as much to ansible as possible, ie puppet and custom scripts replaced by ansible playbooks)
* Production migration start

### Phase 3 (8 weeks)
* Kasm deployment
* NoMachine NX phase-out
* Automation refinement
* Project completion

---

## Risk Mitigation & Next Steps

### Risk Mitigation
* Pilot deployments
* Parallel operations
* Documented procedures
* Rollback capabilities
* Team training program
* Change control process

### Next Steps
1. Create roadmap for Proxmox pilot
2. Begin Ansible framework setup - I think Zaiwen has been experimenting 
3. Schedule OpenShift planning - Already in the works by CAD team
4. Develop initial playbooks - Kyle has handful of scripts he'll be publishing for EIC 
5. Create automation test environment


### Additional Resources
* [Proxmox](https://proxmox.com/en/)
* [OpenShift Documentation Repo](https://docs.openshift.com/)
* [Kasm Workspaces](https://kasmweb.com/)
* [Local Kasm Server  - TBD](localhost)
  


