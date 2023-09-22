name: Bug Report
description: Report a bug
labels: kind/bug
body:
  - type: textarea
    id: problem
    attributes:
      label: What happened?
      description: Please provide details about the bug.
      required: true

  - type: textarea
    id: version
    attributes:
      label: App version
      value: |

        Setting -> About

    validations:
      required: true

  - type: textarea
    id: platform
    attributes:
      label: Platform OS type
     description: Please provide OS type: ios, android, macos, etc.

  - type: textarea
    id: cloudProvider
    attributes:
      label: Cloud provider
      description: Please provide cloud provider info, if the issue is related to kubernetes cluster from a cloud provider.
    validations:
      required: false
