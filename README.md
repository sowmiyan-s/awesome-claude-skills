# Awesome Claude Skills 🚀

A curated collection of highly effective, production-grade custom instructions, system prompts, and skills to supercharge your experience with Claude.

These skills are designed to guide Claude in generating high-quality, professional, and context-specific outputs, steering clear of common AI-generated tropes and templates.

Developed and maintained with ❤️ by **[Sowmiyan](https://github.com/sowmiyan-s)**.

---

## 🛠️ Table of Contents

- [Awesome Claude Skills 🚀](#awesome-claude-skills-)
  - [🌟 Categories & Skills](#-categories--skills)
    - [Web Development](#web-development)
    - [Content Creation](#content-creation)
    - [Software Engineering](#software-engineering)
    - [Business & Productivity](#business--productivity)
  - [🚀 How to Use These Skills](#-how-to-use-these-skills)
  - [🎨 Design Philosophy](#-design-philosophy)
  - [🛠️ Creating Your Own Skill](#-creating-your-own-skill)
  - [🤝 Contributing](#-contributing)
  - [👤 Owner / Creator](#-owner--creator)

---



Each skill `.md` file starts with YAML frontmatter containing the skill's `name` and `description` (useful for auto-triggering/discovery in agentic environments).

---

## 🌟 Categories & Skills

### Web Development (`web-development/`)
* **[Advanced Web Developer (advanced-web-dev.md)](web-development/advanced-web-dev.md)**:
  Transforms Claude into a senior frontend engineer with 30 years of hands-on experience who builds highly polished, premium, and unique web layouts. It specifically targets and eliminates common AI-generated design clichés.

### Content Creation (`content-creator/`)
* **[Blogger Baby (blogger-baby.md)](content-creator/blogger-baby.md)**:
  Writes original, engaging long-or-short-form articles/blog posts ready to publish on Medium, personal blogs, LinkedIn, or newsletters, on ANY topic — technical and non-technical alike.

### Software Engineering (`software-engineering/`)
* *Coming soon! Add your software engineering skills here.*

### Business & Productivity (`business-productivity/`)
* *Coming soon! Add your productivity, business, or resume building skills here.*

---

## 🚀 How to Use These Skills

You can apply these instructions to Claude in several ways:

### Option A: Claude Projects (Recommended)
1. In Claude.ai, open or create a **Project** (available on Pro/Team plans).
2. Click on **Set custom instructions** for the project.
3. Copy the entire markdown content from the desired skill file (e.g., [advanced-web-dev.md](web-development/advanced-web-dev.md)) and paste it there.
4. All new chats created within this project will automatically adhere to this persona and set of rules.

### Option B: Custom Instructions / Profiles
1. Go to your Claude account settings or Profile.
2. Under **Custom Instructions**, paste the skill text.
3. This will apply globally to all your chats.

### Option C: Direct Copy-Paste
For individual chat sessions, you can copy the contents of the file and paste them as the first message:
> "Act according to the following instructions: [Paste content here]"

---

## 🎨 Design Philosophy

Every skill in this repository is crafted with the following core tenets:
* **Realism & Authenticity:** Code and structures should resemble production-ready projects built by seasoned human experts, not templated code generated in bulk.
* **Granular Control:** Prompts are designed to address the nuances that AI models tend to generalize (e.g., specific font pairing, layout choices, animation curves).
* **Zero Placeholders:** Discourages mock files, template placeholders, and generic icons, pushing the model to search or reference real assets.

---

## 🛠️ Creating Your Own Skill

If you want to create your own skill:
1. Create a markdown file (e.g. `my-skill-name.md`) under the appropriate category directory.
2. Define the name and description at the top of the file using YAML frontmatter:
   ```yaml
   ---
   name: your-skill-name
   description: Brief explanation of what it does and when it triggers
   ---
   ```
3. Write clear, structured guidelines for Claude below it.
4. Update the category's local `README.md` and this main `README.md` to link to your new skill.

---

## 🤝 Contributing

Have an awesome Claude skill that you've developed? Contributions are welcome!
1. Fork this repository.
2. Create your feature branch (`git checkout -b skill/my-awesome-skill`).
3. Add your flat markdown skill file under the correct category directory.
4. Ensure your skill has YAML frontmatter at the top:
   ```yaml
   ---
   name: your-skill-name
   description: Brief explanation of what it does
   ---
   ```
5. Commit your changes (`git commit -m "feat: add my-awesome-skill"`).
6. Push to the branch (`git push origin skill/my-awesome-skill`).
7. Open a Pull Request.

---

## 👤 Owner / Creator

This repository is created and maintained by **Sowmiyan**:
* GitHub: [@sowmiyan-s](https://github.com/sowmiyan-s)
