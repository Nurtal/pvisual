# pvisual

A static, single-page OMOP patient data visualizer. Loads multi-file OMOP exports from a directory, renders a radial visit-based visualization with a synchronized vertical timeline. No server, no database — one `index.html` opened via `file://`.

## Language

**Visit**:
A discrete healthcare encounter linking a person to a time-bounded interaction with the system. Maps to `visit_occurrence` in OMOP CDM. Visits are represented as concentric rings, innermost = most recent.
_Avoid_: Encounter, stay, séance

**Measurement**:
A structured numeric or categorical clinical observation (lab result, vital sign, screening score) tied to a visit. Maps to `measurement` in OMOP CDM.
_Avoid_: Event, observation, test, exam

**Ring**:
A concentric circle in the radial visualization representing a single visit. Contains measurement nodes evenly distributed along its arc.
_Avoid_: Circle, orbit

**Node**:
A discrete point on a ring representing one measurement. Clicking a node expands it into an information panel.
_Aavoid_: Dot, marker, point

**Center Node**:
The innermost circle displaying patient identity (person_id, gender, computed age, death status).
_Avoid_: Hub, core, center

**Timeline**:
A vertical sidebar listing all measurements in antéchronological order, with horizontal bars indicating visit membership.
_Avoid_: List, feed, stream

**Concept**:
A standardized clinical concept identified by `measurement_concept_id`. Each unique concept maps to a distinct color in the visualization.
_Avoid_: Type, category, code

**Concept Name**:
The human-readable label for a concept (e.g., "Pain severity - 0-10 verbal numeric rating"). Displayed in node labels and expanded panels.
_Avoid_: Label, description, name

**Source Format**:
The file format of the loaded data. Either OMOP JSON (one `.json` per table with a top-level key) or Synthea CSV (one `.csv` per table with Synthea column names).
_Aavoid_: Input format, data format

**CDM Version**:
The OMOP Common Data Model version of the source data. Detected automatically. Currently supports v5.3 and v5.4.
_Avoid_: Version, schema version
