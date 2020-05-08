# Types of APIs

When designing a new API, you should always ask yourself: *"Who am I designing it for?"*.

Considering the audience of an API will guide its architecture, network configuration, and security measures.

SWIFT normally considers four types of APIs:

| Term | Explanation |
| --- | --- |
| Open APIs | Accessible by anybody who wishes to sign up, and are easily accessible to the wider population on the web and mobile developers, but will often have a vetting service to prevent criminal acts. Open does not necessarily mean free. They should be secure, because they are open to the world. |
| Partner APIs | Open APIs designed for partners. They can hold optimisations requested by partners to make them easier to integrate into their processes. They should be secure, because they are open to the world. |
| Private APIs | Allow an organisation and their contractors to access back-end data and functionality to develop new applications which can then be distributed publicly. They help to reduce development times and internal resources needed by stopping siloed applications from being built from scratch and creating a common center of internal software assets. |
| Internal APIs | Only accessible within an organisation because systems are built and managed by employees. They are ideally not accessible by public Internet. They provide a portal in which cross-departmental projects can be conducted, allowing more flexibility than legacy systems. |

Source: [APIs – Open vs Private vs Internal](https://www.linkedin.com/pulse/apis-open-vs-private-internal-thomas-kaufmann).

This document is applicable to all types of APIs.

The guidelines can differ per type.

The differences are documented as follows:

| Internal | Private | Partner | Open |
| --- | --- | --- | --- |
| Must | Must | Must | Must |

(First row): The type of API it applies to:
- Internal
- Private
- Partner
- Open

(Second row): What is enforced for this type of API:
- Must (*required*) if the criteria must be met.
- Should (*highly recommended*) if the criteria must be met, but it is temporarily allowed to deviate if valid reasons are provided.
- Could (*optional*) if the criteria does not have to be met, but it is preferable to do so.
