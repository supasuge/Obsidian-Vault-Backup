Not all data is equal. Data classification is an important step when organizing security systems and controls. Data classifications is used to identify and organize information that has similar security control needs. Allowing for appropriate access control to be created. When you start thinking about security models, it's important to consider your data classifications. Security models are used to help identify who gets to look at what data. You can't do this without first classifying and organizing your data resources. 

Different organization will use different types of classification shcemes. Depending on the data resources in place. This is also something you will hear a lot when it comes to the government and perhaps especiially the military. 

| **Classification** | **Description**                                                                                                                                                                                    |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Top secret`       | The highest level of data classification. Only a limited number of people will be able to look at data classified as top secret.                                                                   |
| `Secret`           | The exposure of secret information would cause serious damage to national security.                                                                                                                |
| `Confidential`     | The exposure of confidential information would cause damage to national security.                                                                                                                  |
| `Restricted`       | Exposure of restricted data would have undesirable effects.                                                                                                                                        |
| `Official`         | This is information that relates to government business and may not be an indicator of the potential for harm if the information were lost or exposed.                                             |
| `Unclassified`     | Unclassified information can be viewed by everyone. This may include declassified information that was once considered a higher classification, but the threat posed by its exposure has subsided. |
|                    |                                                                                                                                                                                                    |
- Often additional compatments.
- Top secret information is not necessarily available to everyone who has been cleared at a top secret level. Just as not all data is created equal, not all people are created equal. Information sometimes needs to be compartmentalized even beyond these data classification levels. this is not the only way of categorizing data, of course. Organization may have different ways of thinking about their information. 

|            |                                                                                                                                           |
| ---------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| Restricted | Data that is internal or sensitive. Loss or disclosure of this data could cause significant damage to an organization.                    |
| Private    | Data that may also be internal or sensitive, but the loss or disclosure of this data would cause only moderate damage to an organization. |
| Public     | Data that is classified public would incur no damage to an organization if it were disclosed or lost.                                     |

- Classification should always be done based on business requirements and needs. 

### Security Models
Models are used to help enforce access controls. A security model defines who can perform what action on data. It's an extension of the data classificaton levels that an organization had identified. Ex: People who should have access to public data generally shouldn't have access to sensitive information. This isn't a property that goes in both directions, of course. Someone who has acces to secret data should have access to public data but definitely not te othre way around. These are the sorts of relationships that are defined in the models presented in the following:

#### State Machine
The state machine is based on a finite-state machine. A finite-state machine is a way of describing a aset of computation steps. At any point in time,, the machine is either at a single state or in transition between two states. This is an abstract way of evaluating computation models. You can see a simple state machine below, it shows two states, A and B, respectively. In this model, it's possible to transition in both directions


A state machine model is used to identify when the overall security of a system has moved on to a state that isn't secure. This requires that all possible states of the system have been identified. This should include the actions that would be possible to move a system into a particular state and all the possible state transitions. When a system makes a transition that is unexpected or an action results in an unexpected state, this can be monitored and alerted.

This model is far more abstract than the others discussed here since it doesn't actually define any expected behaviors. It's simply a way to model an environment, requiring that someone define behaviors and states.

#### Biba
The biba model is named after the man who developed it in 1975, Kenneth Biba. The goal of the Biba model is data integrity. Because of this, this model is referred to as the Biba Integrity Model. There are three objectives when it comes to ensuring data integrity:  

- Unauthorized parties cannot modify data.
- Authorized parties cannot modify data without specific authorization.
- Data should be true and accurate, meaning it has both internal and external consistency.

The Biba model defines three sets of rules:  

- The Simple Identity Property says a subject at one level of integrity may not read a data object at a lower integrity level.
- The * (star) Identity Property says a subject at one level of integrity may not write to data objects at a higher level of integrity.
- The Invocation Property says a process from below may not request access at a higher level. The process can have access only at its own level or below.

##### Bell–LaPadula

Bell–LaPadula is used in government or military implementations, and the intent is to protect confidentiality. Like Biba, it is a state machine model, meaning that subjects and objects exist in a single state at a point in time. Bell–LaPadula also identifies a _secure state_ so all transitions should move from one secure state to another secure state. A secure state means that subjects are in compliance with objects as defined by security policy. Only appropriate levels of access are allowed within this model because they are defined and can be evaluated.  
  
As noted, Bell–LaPadula is focused on confidentiality rather than integrity. As a result, the model defines properties that are different from those that were defined in the Biba model. The Bell–LaPadula properties are defined as follows:  

- The Simple Security Property says that a subject at one security level may not read an object at a higher security level.
- The * (star) Property says that a subject at one security level may not write to an object at a lower security level.
- The Discretionary Security Property uses an access matrix to indicate discretionary access.

Bella LaPadula provides for two mandatory access control rules.