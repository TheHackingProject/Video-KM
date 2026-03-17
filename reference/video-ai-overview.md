---
title: "Video-AI Overview"
type: documentation
diataxis: reference
status: published
area: video-ai
tags:
  - reference
  - video-ai
  - thp
  - overview
created: 2026-03-12
updated: 2026-03-12
related:
  - "[[00-architecture]]"
  - "[[runbooks/remotion]]"
  - "[[explanation/video-ai-vision]]"
---

# Video-AI Overview

Reference: what Video-AI is, who it is for, and where it fits.

## Role within THP

Video-AI is the monorepo and tooling used to produce and evolve **pedagogical videos** for The Hacking Project (THP). It provides the pipeline from Remotion compositions to videos consumed by the THP learning platform.

## Target audience

- **Primary**: THP learners (web development courses).
- **Secondary**: THP team and contributors who create, edit, or improve course videos.

## Types of videos

- **Web development courses**: concept explanations, code demos, recaps.
- Videos are generated from React/Remotion compositions in this repo and integrated into the THP app (or equivalent consumer).

## Where videos are used

The videos produced by Video-AI are consumed by the THP learning platform. The exact integration (embed, CDN, app) is documented in the THP app or platform docs; this repo focuses on authoring and rendering the video assets.

## See also

- [00-architecture](00-architecture.md) – Monorepo structure and Remotion app layout.
- [runbooks/remotion](runbooks/remotion.md) – How to work with Remotion compositions.
- [explanation/video-ai-vision](explanation/video-ai-vision.md) – Long-term vision and v1/v2/v3 roadmap.
