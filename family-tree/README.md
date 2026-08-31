# Cây Gia Phả

An interactive, browser-based family tree built with React, TypeScript, Vite, React Flow, and Dagre. The interface is written in Vietnamese and presents family relationships as a navigable graph with searchable members and locally editable profiles.

This directory contains the application source. For repository-level documentation, see the [main README](../README.md).

## Overview

The application turns a static family data set into a visual top-to-bottom family tree. React Flow provides the interactive canvas, while Dagre calculates the initial node positions. Each node represents an individual or couple and can open a profile sidebar containing dates, a biography, achievements, and an optional photo.

Profile changes are saved in the browser with `localStorage`. They are not written back to the source file, sent to a server, or synchronized between devices.

## Features

- Interactive family tree canvas with pan, zoom, fit-view controls, and a background grid
- Automatic top-to-bottom graph layout powered by Dagre
- Custom person and couple cards with visual relationship indicators
- Search by member name
- Filters for profiles that contain a biography or birth date
- Profile sidebar for viewing and editing personal details
- Local photo uploads stored as data URLs in the browser
- Browser persistence for edited profile information
- Responsive controls and panels for desktop and smaller screens
- Vietnamese onboarding tour that explains navigation, filters, profiles, and canvas controls
- Color-coded relationship edges and a legend for the represented family branches

## Tech stack

| Area | Technology |
| --- | --- |
| UI framework | React 19 |
| Language | TypeScript |
| Build tool | Vite 6 |
| Styling | Tailwind CSS 4 |
| Graph canvas | React Flow |
| Automatic layout | Dagre |
| Guided tour | React Joyride |
| Icons | Lucide React |
| Animation dependency | Motion |
| Utility classes | clsx and tailwind-merge |

The dependency manifest also contains packages inherited from the original project setup. The current frontend source does not use a server, database, or Gemini request at runtime.

## Project structure

```text
family-tree/
├── index.html
├── metadata.json
├── package.json
├── package-lock.json
├── tsconfig.json
├── vite.config.ts
└── src/
    ├── App.tsx
    ├── main.tsx
    ├── index.css
    ├── components/
    │   ├── FamilyNode.tsx
    │   ├── FilterPanel.tsx
    │   └── ProfileSidebar.tsx
    ├── context/
    │   ├── FilterContext.tsx
    │   └── ProfileContext.tsx
    ├── data/
    │   └── familyTree.ts
    └── utils/
        ├── cn.ts
        └── layout.ts
```

### Main modules

- `src/App.tsx` composes the graph, filters, profile sidebar, and onboarding tour.
- `src/data/familyTree.ts` defines the initial people, nodes, edges, and helper lookup.
- `src/components/FamilyNode.tsx` renders each individual or couple and applies filter highlighting.
- `src/components/ProfileSidebar.tsx` displays and edits the selected person's local profile.
- `src/components/FilterPanel.tsx` manages name and profile-completeness filters.
- `src/context/ProfileContext.tsx` stores profile state and persists it to `localStorage`.
- `src/context/FilterContext.tsx` provides filter state to the interface.
- `src/utils/layout.ts` runs Dagre and maps its coordinates into React Flow positions.

## Getting started

### Prerequisites

Install Node.js and npm. The repository does not declare an exact Node.js version.

### Install

From this directory:

```bash
npm install
```

### Start the development server

```bash
npm run dev
```

Vite is configured to listen on all interfaces and use port `3000`. Open the local address printed in the terminal.

No environment variable is required by the current application behavior. Although `vite.config.ts` maps `GEMINI_API_KEY` and the dependency list includes `@google/genai`, the checked-in application source does not make a Gemini API request.

## Available scripts

| Command | Purpose |
| --- | --- |
| `npm run dev` | Start Vite in development mode on port 3000 |
| `npm run build` | Create a production build in `dist/` |
| `npm run preview` | Serve the production build locally for inspection |
| `npm run lint` | Run the TypeScript compiler in no-output mode |
| `npm run clean` | Remove the generated `dist/` directory |

There is currently no automated test command in `package.json`.

## Build and preview

Create a production bundle:

```bash
npm run build
```

Preview that bundle locally:

```bash
npm run preview
```

The generated `dist/` directory is build output and can be removed with:

```bash
npm run clean
```

## Data model

The family tree has two related layers of data.

### Relationship data

`src/data/familyTree.ts` contains the people displayed in the graph and the node-to-node relationships used by React Flow. A person has an identifier, display name, optional role, optional gender marker, and an optional flag for the central member.

A family node groups one or more people and may be marked as a couple or as the center of the tree. Edges connect those nodes and determine the relationships visible on the canvas.

### Profile data

Profile details are separate from the relationship graph. The editable profile supports:

- Photo
- Birth date
- Death date
- Biography
- Achievements

These details are stored under the `familyProfiles` key in the current browser's `localStorage`. Completing or skipping the onboarding tour sets the `tourCompleted` key.

## Customizing the tree

To adapt the app for another family:

1. Open `src/data/familyTree.ts`.
2. Replace or extend the person records with unique IDs.
3. Update the family nodes so each person appears in the intended individual or couple card.
4. Update the edges to connect parent, child, and branch nodes.
5. Run `npm run lint` to catch TypeScript errors.
6. Start `npm run dev` and inspect the graph layout at multiple screen sizes.

The layout utility currently uses fixed node dimensions and a top-to-bottom direction. Spacing and direction can be adjusted in `src/utils/layout.ts`.

## Interaction guide

- Drag the canvas to move around the tree.
- Use the mouse wheel, trackpad, or React Flow controls to zoom.
- Select a person's photo or name to open the profile sidebar.
- Choose **Chỉnh sửa** to edit the selected profile, then **Lưu thay đổi** to save it locally.
- Enter a name in **Bộ lọc & Tìm kiếm** to emphasize matching members.
- Enable the biography or birth-date filters to emphasize profiles with those fields.
- On smaller screens, use the arrow in the top panel to expand or collapse the legend and filters.

Filtering dims nonmatching people rather than removing them, so the overall tree structure remains visible.

## Local persistence and privacy

This project includes family relationship data in the checked-in source and saves edited profile details in the browser. Keep these behaviors in mind:

- A public repository makes committed names and relationships publicly visible.
- Browser edits are specific to the browser profile and device being used.
- Clearing site data or browser storage removes locally edited profiles.
- Uploaded photos are converted to data URLs and stored in `localStorage`, which has limited capacity.
- There is no authentication, access control, encryption layer, cloud backup, or multi-device synchronization in the current implementation.
- Avoid committing private dates, biographies, photos, or other sensitive family information unless everyone involved has agreed to publication.

For a public fork or demonstration, replace real records with fictional or consented sample data.

## Current limitations

- Family relationships are defined in source code rather than through an editor.
- Runtime profile edits do not update `familyTree.ts`.
- Connections added on the React Flow canvas are not persisted after a reload.
- There is no import or export workflow.
- There is no backend or shared account system.
- Search checks names only.
- Profile photos depend on browser storage limits.
- The repository does not include automated tests.
- The app has no offline synchronization or conflict handling.

## Troubleshooting

### Port 3000 is already in use

Stop the process using port 3000 or run Vite with a different port:

```bash
npm run dev -- --port 3001
```

### Profile changes disappeared

Profile edits live only in `localStorage`. They can disappear after clearing site data, switching browsers, using a different device, or opening the app under a different origin.

### The tree layout looks crowded

Review node and edge definitions in `src/data/familyTree.ts`, then adjust `nodeWidth`, `nodeHeight`, `nodesep`, or `ranksep` in `src/utils/layout.ts`.

### TypeScript validation fails

Run:

```bash
npm run lint
```

Then address the file and type errors reported by `tsc`.

## Project status

The current repository contains a functional client-side family-tree viewer and profile editor. It is best understood as a personal frontend application or prototype: its core visualization and local editing flows are implemented, while server-backed persistence, authentication, automated testing, and data-management tooling are not present.

## License

No license file is included in this repository.
