[[def: finalization-identifier, finalization identifier]]

~ An optional AID carried in a dossier's `fi` attribute. When present and non-null, it names the AID whose key event log a verifier should expect to carry a finalization event — the event that collects the threshold-satisfying endorsements of a joint issuance in one predictable place. When absent or null, no finalization event is promised and a verifier gathers endorsements from the participants' individual KELs.
