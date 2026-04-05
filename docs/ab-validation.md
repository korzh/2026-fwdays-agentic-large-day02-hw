## A/B Test: architecture.mdc

- **Prompt**: "Create a new component for displaying element coordinates"
- **Result A (rule ON)**: uses actionManager, named export, strict types
- **Result B (rule OFF)**: uses useState, default export, loose types
- **Conclusion**: Rule effectively prevents Zustand/Redux suggestions