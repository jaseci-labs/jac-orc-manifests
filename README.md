# JAC ORC MANIFESTS
- This Templates will be used by Jac-Orc Service spawned by jac-orc.yaml


# HOW TO CREATE TEMPLATE
## Folder structure
```python
service-name/ # your module name
│
├── latest/ # version that keeps updating
│   │
│   ├── depenendies/ # dependency yamls that will be automatically applied to install the module. Usually doesn't have placeholders
│   │   │
│   │   ├── dependency-1.yaml # any name
│   │   │
│   │   └── dependency-2.yaml # any name
│   │
│   ├── extras/ # any optional yamls that user can apply manually
│   │   │
│   │   ├── extra-1.yaml # any name
│   │   │
│   │   └── extra-2.yaml # any name
│   │
│   ├── service-manifests-1.yaml # any additional yaml to fully run the service. Usually have the placeholders
│   │
│   ├── service-manifests-2.yaml # any additional yaml to fully run the service. Usually have the placeholders
│   │
│   └── service-manifests-3.yaml # any additional yaml to fully run the service. Usually have the placeholders
│
└── 0.1.0/ # specific version
    │
    ├── depenendies/ # dependency yamls that will be automatically applied to install the module. Usually doesn't have placeholders
    │   │
    │   ├── dependency-1.yaml # any name
    │   │
    │   └── dependency-2.yaml # any name
    │
    ├── extras/ # any optional yamls that user can apply manually
    │   │
    │   ├── extra-1.yaml # any name
    │   │
    │   └── extra-2.yaml # any name
    │
    ├── service-manifests-1.yaml # any additional yaml to fully run the service. Usually have the placeholders
    │
    ├── service-manifests-2.yaml # any additional yaml to fully run the service. Usually have the placeholders
    │
    └── service-manifests-3.yaml # any additional yaml to fully run the service. Usually have the placeholders
```
## Placeholder Syntax
### Generic `$g{placeholder_name:optional_default_value}`
```yaml
spec:
  template:
    metadata:
      labels:
        string: $g{placeholder1:"your-string"}
        string_with_quoutes: "$g{placeholder1:"your-string"}"
        number: $g{placeholder1:123445}
        number_converted_to_string: "$g{placeholder1:123445}"
        string_array: $g{placeholder1:["test", "test"]}
        string_number: $g{placeholder1:[1, 2]}
        string_array_converted_to_string: "$g{placeholder1:["test", "test"]}"
```
### New Line Array `$a{placeholder_name:optional_default_value}`
- This is used for multilined scripts
```yaml
spec:
  template:
    spec:
      containers:
        - name: testing
          args:
            - |
              $a{
                args:[
                  "mkdir app && cd app",
                  "apt-get update && apt-get install -y --no-install-recommends git",
                  "git clone --depth 1 https://github.com/Jaseci-Labs/littleX.git .",
                  "pip install -r ./littleX_BE/requirements.txt openai",
                  "jac serve ./littleX_BE/littleX.jac"
                ]
              }
```
### New Line Dict `$d{placeholder_name:optional_default_value}`
- This is used for multilined key value pair
```yaml
data:
  $d{env-vars:{"key1":"val1"}}

# or

# this is to handle auto formatting issue
# incase you have extension to format yaml
data:
  - $d{env-vars:{"key1":"val1"}}
```