# Solo Developer SDLC Checklist

## 1. Planning & Requirements (Output - Software Requirements Specification.md)

*Prompt: I have a project to create a {name of app}. Basically, this app will {bullet item goals}. We want to create a clean Software Requirements Specification (SRS) document that includes all the elements from the checklist below and the provided template. Ask me any questions before you produce the SRS document.*

- [ ] Define project goals and objectives
- [ ] Identify target users and their needs
- [ ] List core features (Minimum Viable Product - MVP scope) "Version 1"
- [ ] List what can wait / nice-to-have features (post-MVP) "Version 2"
- [ ] Define success metrics
- [ ] Estimate timeline and set milestones
- [ ] Identify potential risks and constraints
- [ ] Choose tech stack and architecture

## 2. Design (Output - Design.md )

*Prompt: We will use the provided Software Requirements Specification.md to complete the checklist below. All will be in one file, Design.md, so even though each task has an output, it's just appended to the same file, except for the last file, README.md, which will be a separate file summarising everything.*

- [ ] Create detailed Use Case Diagrams (Output: MD file with both Text-Based UML Style and Mermaid)
- [ ] Create detailed class diagrams (Output: MD file with both Text-Based UML Style + Mermaid)
- [ ] Create detailed Sequence diagrams (Output: MD file with both Text-Based UML Style + Mermaid)
- [ ] Sketch wireframes or mockups (Output: Rough sketch using ASCII MD file)
- [ ] Design a detailed database schema (Output: MD file with both text format + Mermaid)
- [ ] Plan API structure and endpoints (Output: MD file)
- [ ] Document architecture decisions and create System Architecture Diagram (Output: MD file with both text format + Mermaid)
- [ ] Consider scalability and performance needs (Output: MD file)
- [ ] Plan authentication and authorisation approach (Output: MD file)
- [ ] Identify third-party services needed (Output: MD file)
- [ ] Create project structure based on the below structure (Output: MD file)
  root/
  |--app/
  |--docs/
  |--scripts/
  |	|--examples/
  |--logs/
  |--tests/
- [ ] Create initial documentation README.md

## 3. Development Setup

- [ ] Initialise version control (Git)
- [ ] Set up remote repository (GitHub/GitLab)
- [ ] Set up development environment
- [ ] Install dependencies and tools
- [ ] Configure logging, linting and formatting
- [ ] Set up environment variables management

## 4. Implementation

- [ ] Break work into small, manageable tasks.
- [ ] Each Task in its own git branch
- [ ] Follow coding standards and conventions
- [ ] Write clean, self-documenting code
- [ ] Commit changes regularly with clear messages
- [ ] Implement one feature at a time
- [ ] Add inline comments for complex logic
- [ ] Keep functions small and focused
- [ ] Handle errors gracefully

## 5. Testing

- [ ] Write unit tests for critical functions
- [ ] Write example cases
- [ ] Perform integration testing
- [ ] Test "happy path" and "unhappy path" (edge cases and error scenarios)
- [ ] Test on different browsers/devices (if applicable)
- [ ] Verify all user flows work end-to-end
- [ ] Test with realistic data volumes
- [ ] Security testing (basic vulnerabilities)
- [ ] Performance testing under load

## 6. Documentation

- [ ] Update README with setup instructions
- [ ] Document API endpoints and parameters
- [ ] Add code comments where needed
- [ ] Create user guide or usage examples
- [ ] Document configuration options
- [ ] Note any known issues or limitations
- [ ] Include troubleshooting section
- [ ] Add changelog for version tracking

## 7. Deployment Preparation

- [ ] Set up production environment
- [ ] Configure production database
- [ ] Set up CI/CD pipeline (optional but recommended)
- [ ] Configure environment-specific settings
- [ ] Set up monitoring and logging
- [ ] Prepare backup strategy
- [ ] Configure domain and SSL certificate
- [ ] Test deployment in staging environment

## 8. Launch

- [ ] Perform final testing in production-like environment
- [ ] Deploy to production
- [ ] Verify all features work in production
- [ ] Monitor error logs and metrics
- [ ] Create initial backup
- [ ] Announce launch (if applicable)
- [ ] Monitor user feedback and issues

## 9. Maintenance & Iteration

- [ ] Monitor application health and performance
- [ ] Review error logs regularly
- [ ] Address bug reports promptly
- [ ] Collect and prioritize user feedback
- [ ] Plan next iteration or features
- [ ] Keep dependencies updated
- [ ] Maintain documentation
- [ ] Regular security audits
- [ ] Database maintenance and optimization
- [ ] Backup verification

## 10. Retrospective

- [ ] Review what went well
- [ ] Identify what could be improved
- [ ] Document lessons learned
- [ ] Update process for next project
- [ ] Celebrate your wins!

---

## Tips for Solo Developers:

- Don't skip planning, even if you're eager to code
- Keep it simple—avoid over-engineering
- Automate repetitive tasks early
- Use issue tracking even for personal projects
- Take breaks to avoid burnout
- Don't aim for perfection—ship and iterate
- Back up your work frequently
- Document as you go, not after
