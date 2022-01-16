# Cost effective resources
- ### It is a important aspect when designing solutions in the cloud
- ### AWS provides several services that working together can make the best to keep budgets under control
- ### Topics:
    - ### [Reserved instances](#reserved-instances)
    - ### [Billing and cost management](#billing)
    - ### [AWS organizations](#organizations)

## Note:
- ### Tags are a great tool for classifying resources. For example: organization, environment, and project
## Technical requirements
- ### IAM users will require special permissions in order to interact with the billing and cost management console
- ### [Ref](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/control-access-billing.html)

## <h2 id="reserved-instances">Reserved instances (RI)</h2>
- ### RI have 2 objectives:
    - ### The ability to reserve capacity for when you need it
    - ### Creating savings in your account
- ### RI work by purchasing instances of specific types
- ### They will billed with the RI model

### Standard reserved instances
- ### Specifying how long you will reserve the instance (can choosen for 1 or 3 year) -> discount
- ### Paying with 3 options:
    - ### All upfront (best price)
    - ### Partial upfront (discount)
    - ### No upfront (pay on a per month basic)

### Convertible reserved instances
- ### One m1.xlarge can be changed for 2 m1.large, 4 m1.medium, 8 m1.smal
- ### Reserved instance can also be resold in the AWS MarketPlace
- ### [Ref](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/apply_ri.html) 

## <h2 id="billing">Billing and cost management</h2>
- ### Dashboard provides a place to manage the economics of your account

## <h2 id="organizations">AWS organization</h2>
- ### Are a way to centrally manage account hierarchies that resemble organizational structures that make sense from the management, security and billing perspectives