# Request Escrow Invoices

## What problem does Escrow solve?

The Escrow feature intends to increase the confidence for first-time collaboration between contractor and client.

The contractor creates and sends an Invoice to their client, asking them to deposit the invoice funds to the Escrow contract so they can be reassured that the money for the job is available.

The contractor starts their work and then notifies the client when the job is done.

If the client is satisfied with the job, they release the funds to the contractor.

# Conflict resolution

All conflict resolution is done by the smart contract. Request Network does not act as a middleman and cannot make any decission on your part.

Please note that the smart contract is still in beta and hasn't been reviewed so we are willing to provide refunds in case of any funds loss due to bugs up to $5,000.

## Client becomes unresponsive

It is possible that the client loses their private keys and they can't release the money to the contractor.
In that case, the contractor can _initiate an emergency claim_ and this will put the escrow in emergency state.
After the emergency claim is raised, there is a period of 6 months after which the contractor will be able to retrieve the money without the need for approval from the client.

However, if the client recovers their private keys in the meantime, they can revert the emergency claim within the 6 months period.

## Client is not satisfied with the job

If the client is not satisfied with the job performed by the contractor, they have the option to _freeze_ the escrow.

Freezing the escrow means that the money will remain in the contract for 12 months and after that the client will be able to withdraw the full amount.

We have put the 12 month period in order to incentivise clients to try their very best to reach an agreement with their contractor before using this feature, however they must have the option to retrieve their money as a last resort.

Warning: this action is irreversible. Once an escrow has been frozen, it cannot be unfrozen, not even by the client, the money will remain locked for 12 months.
