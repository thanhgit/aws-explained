# Terraform common

## Supported types
- Strings
- Maps
- Lists
- Booleans
```text
variable "key" {
    type = "string"
    default = "value"
}

variable "multilineString" {
    type = "string"
    default  = <<EOF
        Line1 
        Line2
    EOF
}
```