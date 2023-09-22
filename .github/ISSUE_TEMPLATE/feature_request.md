---
name: Feature request
about: Request a feature for this project
title: ''
labels: ''
assignees: ''

---

name: Feature request
description: Provide details for a feature
labels: kind/enhancement
body:
  - type: textarea
    id: feature
    attributes:
      label: What would you like to be added?
      description: |
        Describe functionality of the feature you request, as much details as you can.
      required: true

  - type: textarea
    id: rationale
    attributes:
      label: Why is this needed?
    validations:
      required: false