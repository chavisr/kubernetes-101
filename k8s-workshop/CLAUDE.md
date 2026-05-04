# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

A Marp presentation for a 3-hour hands-on Kubernetes basics workshop. The single source file is `k8s-workshop.md`.

## Rendering

```bash
# Live preview in browser
marp --preview k8s-workshop.md

# Export to HTML
marp k8s-workshop.md -o k8s-workshop.html

# Export to PDF
marp k8s-workshop.md --pdf -o k8s-workshop.pdf
```

## Slide Structure

- All CSS is embedded in a single `<style>` block at the top of the file
- Three custom section classes: `section.lead` (title/end), `section.chapter` (dark divider), `section.break` (rest slide)
- Per-slide overrides use `<style scoped>` blocks placed before the slide's heading
- Slides are separated by `---`

## Theme & Design Decisions

- Theme: minimal — white background, `Noto Sans JP` font, light weights
- Code blocks: light gray background (`#f4f4f4`), monospace font, bordered
- All app examples use `crccheck/hello-world` image (port **8000**) with resource name `hello`
- Students use **Docker Desktop** with Kubernetes enabled — no minikube or kind
- `ingress-nginx` / `ingressClassName: nginx` are controller references, not app names — do not rename these

## Content Conventions

- Commands use actual resource names (`hello`, `hello-deployment.yaml`) not placeholders like `<name>`
- Scoped style pattern for overflowing slides: `h2 { margin-bottom: Npx; } pre { margin: Npx 0; padding: Npx Npx; font-size: Nem; }`
