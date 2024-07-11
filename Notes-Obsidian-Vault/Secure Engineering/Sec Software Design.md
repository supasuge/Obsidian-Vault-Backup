## Table of Contents

    - [Array Bounding](#Array\Bounding)
    - [Data canaries](#Data\canaries)

General Terms:
- Thick Client
- Thin Client
	Distinction is in the level of processing and business logic that occurs on the client machine. A thick client is one that performs a significant amount of the business logic and resource consumption on the client machine. A thing client uses the client machine mostly as an interface to the server or system that handlers the business logic and contians the resource that will be used for processing.


### Array Bounding
Any data that is ever written to an array variable needs to have bounds testing to make sure it does no precede or surpass the allowedlength of the array.


### Data canaries
Value loated at the end of an ssigned buffer lenght. Verifying that this value is still correct is a means of alidating that an overflow has not occurred. These come in many forms, but essential elements have assinged when the mem is allocated


194.160.218.12
shadowserver2.csirt.**upjs.sk**
