# Users and Attesters

## Actors in Unirep

1. **User**: there are users who receive reputation and prove received reputation
   * users sign up by calling `userSignUp` in Unirep contract
   * user's `identityCommitment` is revealed at this time and it will be recorded in the contract to prevent double signup
   * the identity commitment will not reveal the actual identity of the user but at the same time allow user to prove identity in the cirucit
2. **Attester**: there are attesters who give attestations to users and the attestations become the users' reputation
   * attesters sign up by calling `attesterSignUp` in Unirep contract
   * attesters would be given `attesterId` by the order they sign up, `attesterId` begins with `1`
   * attester records and attestation records are public and so everyone can see which attester submits which attestation to the Unirep contract
