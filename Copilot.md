# Developer Guide: Building and Rolling Out a GitHub Copilot Extension

This guide walks developers and platform teams through designing, building, configuring, and rolling out a GitHub Copilot Extension backed by a GitHub App and a publicly accessible service.

References:
- [Creating a GitHub Copilot Extension](https://docs.github.com/en/copilot/how-tos/use-copilot-extensions/create-a-copilot-extension)
- [Configuring your server to host your GitHub Copilot extension](https://docs.github.com/en/copilot/how-tos/use-copilot-extensions/create-a-copilot-extension/host-your-extension)
- [Creating a GitHub App for your GitHub Copilot Extension](https://docs.github.com/en/copilot/how-tos/use-copilot-extensions/create-a-copilot-extension/create-github-app)
- [Configuring your GitHub App for your GitHub Copilot extension](https://docs.github.com/en/copilot/how-tos/use-copilot-extensions/create-a-copilot-extension/configure-app-for-extension)
- [Enabling developers to use GitHub Copilot](https://docs.github.com/en/copilot/tutorials/roll-out-at-scale/enable-developers)
- [GitHub Copilot Terms](https://docs.github.com/en/site-policy/github-terms/github-terms-for-additional-products-and-features#github-copilot)
- [Best practices for using Copilot](https://docs.github.com/en/copilot/get-started/best-practices)
- Note on support scope: [Getting help from GitHub Support about GitHub Actions](https://docs.github.com/en/actions/how-tos/get-support)

---

## 1. Purpose and scope

- Build a Copilot Extension that securely connects Copilot to your organization’s systems via a GitHub App and a server you operate.
- Provide a clear, repeatable process for development, security review, deployment, and organization-wide rollout.

This guide references official GitHub documentation for Copilot Extensions and enterprise rollout enablement. See:
- [Creating a GitHub Copilot Extension](https://docs.github.com/en/copilot/how-tos/use-copilot-extensions/create-a-copilot-extension)
- [Enabling developers to use GitHub Copilot](https://docs.github.com/en/copilot/tutorials/roll-out-at-scale/enable-developers)

## 2. Audience

- Platform engineers, internal developer platform (IDP) teams
- Application developers integrating domain workflows with Copilot
- Security, compliance, and operations teams

## 3. Outcomes

By the end of this guide you will:
- Host a secure, internet-accessible service for your extension.
- Create and configure a GitHub App tied to your Copilot Extension.
- Connect Copilot to your extension and validate end-to-end behavior.
- Plan enablement and adoption across your engineering org.

References:
- Hosting: [Configuring your server to host your GitHub Copilot extension](https://docs.github.com/en/copilot/how-tos/use-copilot-extensions/create-a-copilot-extension/host-your-extension)
- App setup: [Creating a GitHub App for your GitHub Copilot Extension](https://docs.github.com/en/copilot/how-tos/use-copilot-extensions/create-a-copilot-extension/create-github-app)
- App configuration: [Configuring your GitHub App for your GitHub Copilot extension](https://docs.github.com/en/copilot/how-tos/use-copilot-extensions/create-a-copilot-extension/configure-app-for-extension)
- Rollout: [Enabling developers to use GitHub Copilot](https://docs.github.com/en/copilot/tutorials/roll-out-at-scale/enable-developers)

## 4. Prerequisites

- An organization on GitHub with the ability to create and manage GitHub Apps.
- Access to deploy and operate a secure, publicly reachable service (HTTPS).
- Internal processes for security review and incident response.

See:
- [Creating a GitHub Copilot Extension](https://docs.github.com/en/copilot/how-tos/use-copilot-extensions/create-a-copilot-extension)

## 5. Key concepts

- Copilot Extension: An integration that lets Copilot securely call your domain logic through a GitHub App and your hosted service.
- GitHub App: The identity and permission model Copilot uses to interact with your resources.
- Hosted service: Your API or agent service reachable from the internet.

Learn more:
- [Creating a GitHub Copilot Extension](https://docs.github.com/en/copilot/how-tos/use-copilot-extensions/create-a-copilot-extension)

## 6. Architecture overview

High-level flow:
1. Copilot invokes your extension when relevant to a user’s prompt.
2. The request is routed through your configured GitHub App context.
3. Your hosted service processes the request and returns a response that Copilot can use.

Reference:
- [Creating a GitHub Copilot Extension](https://docs.github.com/en/copilot/how-tos/use-copilot-extensions/create-a-copilot-extension)

## 7. Step-by-step implementation

### 7.1 Host your extension service
- Provide an HTTPS endpoint accessible from the public internet.
- Prepare secure configuration, logging, and monitoring.
- Validate health endpoints and readiness checks.

Follow:
- [Configuring your server to host your GitHub Copilot extension](https://docs.github.com/en/copilot/how-tos/use-copilot-extensions/create-a-copilot-extension/host-your-extension)

### 7.2 Create a GitHub App for the extension
- Create a GitHub App in your organization that will represent the extension.
- Record keys/credentials per the guide and store them securely.

Steps:
- [Creating a GitHub App for your GitHub Copilot Extension](https://docs.github.com/en/copilot/how-tos/use-copilot-extensions/create-a-copilot-extension/create-github-app)

### 7.3 Configure the GitHub App for your extension
- Set required permissions and any callback URLs or webhook settings as prescribed.
- Associate the App with your Copilot Extension configuration so Copilot can call it.

See:
- [Configuring your GitHub App for your GitHub Copilot extension](https://docs.github.com/en/copilot/how-tos/use-copilot-extensions/create-a-copilot-extension/configure-app-for-extension)

### 7.4 Connect Copilot to your extension
- Complete any remaining configuration in the “Create a Copilot Extension” workflow.
- Test with sample prompts to verify requests reach your service and responses render correctly in Copilot.

Reference:
- [Creating a GitHub Copilot Extension](https://docs.github.com/en/copilot/how-tos/use-copilot-extensions/create-a-copilot-extension)

## 8. Security, privacy, and compliance

- Review Copilot terms and best practices with your legal and security teams.
- Ensure principle-of-least-privilege permissions on your GitHub App.
- Implement audit logging and data handling aligned with your policies.

Read:
- [GitHub Copilot Terms](https://docs.github.com/en/site-policy/github-terms/github-terms-for-additional-products-and-features#github-copilot)
- [Best practices for using Copilot](https://docs.github.com/en/copilot/get-started/best-practices)

Note: GitHub Support does not guarantee correctness of Copilot outputs; validate responses meet your requirements. See:
- [Getting help from GitHub Support about GitHub Actions](https://docs.github.com/en/actions/how-tos/get-support)

## 9. Developer experience and rollout

- Plan enablement and change management to drive adoption.
- Provide example prompts/playbooks tailored to your domain workflows.
- Integrate agentic AI into your SDLC where it adds measurable value.

Guidance:
- [Enabling developers to use GitHub Copilot](https://docs.github.com/en/copilot/tutorials/roll-out-at-scale/enable-developers)

## 10. Testing, observability, and operations

- Establish environments (dev/staging/prod) for your hosted service.
- Implement telemetry for request rates, latency, errors, and usage patterns.
- Define SLOs and incident runbooks for service degradations impacting Copilot experiences.

Reference the hosting and app configuration guides for required endpoints and operational considerations:
- [Configuring your server to host your GitHub Copilot extension](https://docs.github.com/en/copilot/how-tos/use-copilot-extensions/create-a-copilot-extension/host-your-extension)
- [Configuring your GitHub App for your GitHub Copilot extension](https://docs.github.com/en/copilot/how-tos/use-copilot-extensions/create-a-copilot-extension/configure-app-for-extension)

## 11. Troubleshooting and support

- Validate network connectivity and TLS configuration for your hosted service.
- Check GitHub App configuration (permissions, URLs) against the official guide.
- If you need help determining support scope, you can open a ticket; note Copilot outputs are out of support scope.

See:
- [Creating a GitHub Copilot Extension](https://docs.github.com/en/copilot/how-tos/use-copilot-extensions/create-a-copilot-extension)
- [Getting help from GitHub Support about GitHub Actions](https://docs.github.com/en/actions/how-tos/get-support)

## 12. Checklist

- [ ] Public, secure HTTPS endpoint ready
- [ ] GitHub App created for the extension
- [ ] App configured per extension requirements
- [ ] Copilot Extension connected and tested end-to-end
- [ ] Security review completed; permissions minimized
- [ ] Observability and runbooks in place
- [ ] Enablement plan for teams and example prompts/tutorials published

References:
- [Creating a GitHub Copilot Extension](https://docs.github.com/en/copilot/how-tos/use-copilot-extensions/create-a-copilot-extension)
- [Enabling developers to use GitHub Copilot](https://docs.github.com/en/copilot/tutorials/roll-out-at-scale/enable-developers)

---

If you need this guide tailored to your stack or security requirements, share details about your runtime, hosting, and target developer workflows, and we’ll adapt the steps while staying aligned with:
- [Creating a GitHub Copilot Extension](https://docs.github.com/en/copilot/how-tos/use-copilot-extensions/create-a-copilot-extension)
- [Enabling developers to use GitHub Copilot](https://docs.github.com/en/copilot/tutorials/roll-out-at-scale/enable-developers)
- [Best practices for using Copilot](https://docs.github.com/en/copilot/get-started/best-practices)
