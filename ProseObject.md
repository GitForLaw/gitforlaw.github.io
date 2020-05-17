# Stage1 - validate the basic document rendering concept:

Narrative version of the core paradigm (currently PoC-ed in perl, and version with edit-in-place interface and navigation shown at https://www.youtube.com/watch?v=4ZfsyTPYFIA):

1. A Tree of Records

   Given a tree of Records based at a "Root" where each Record is:

   1.  a hash of key/values and

   1.  a list of other Records referenced with/without Prefixes


1. Record Features

   Assuming

2.1.sec=Each Record has a unique name

2.2.sec=The keys are more or less any string, case-sensitive, without leading or trailing whitespace

2.3.sec=The values are strings and may contain {Entities} and HTML markup.

2.=[G/Z/ol/s3]

3.Ti=Rendering

3.0.sec=Given an input of:

3.1.sec=the name of a Root1

3.2.sec=the name of a Record1

3.3.sec=the name of a Key1

3.4.sec=a desired View

3.00.0.sec=Return a string which is:

3.00.1.sec=The Value of the Key1, recursively expanding any {Entities}

3.00.2.sec=By replacing the {Entities} with the value of a matching Key

3.0.=[G/Z/ol/s2]

3.=[G/Z/ol/s4]

4.Ti=Matching an Entity with a Key

4.1.Ti=General

4.1.1.sec=Search for the Key in Record1, or in a linked Record, inquiring depth-first

4.1.2.sec=The search for a match always starts at the top, from Record1. (Enabling modification by overides.)

4.1.=[G/Z/ol/s2]

4.2.Ti=Prefixing:

4.2.1.sec=Rule: If the {Entitiy} is in a Prefixed Record, then treat the {Entity} as being {PrefixEntity}. E.g. if the Record2 is referenced as Buyer.=[Record2], then {Name} in a value in Record2 is treated as {Buyer.Name}. Recursively. So if {Name} is in Record3 and Record2 references it as President.=[Record3], then {Name} is treated as {Buyer.President.Name}.

4.2.2.sec=Deprefixing: If there is no match for {Buyer.President.Name}, then look for {Buyer.Name} and then for {Name}. (Note - this deprefixing has nothing to do with the periods, it works the same for {BuyerPresidentName}, if the references are Buyer=[Record2], President=[Record3]. The periods are characters like any other.) (Note also that the deprefixing starts from the rightmost Prefix.)

4.2.=[G/Z/ol/s2]

4.3.Ti=Failure to Find a Match

4.3.sec=Leave any unmatched {Entities} in the string (optionally marking them in red).

4.=[G/Z/ol/3]

5.Ti=Metadata Wrapping Each Substitution

5.sec=Depending on the View, wrap each substitution with metadata about its source (Record and Key) to aid navigation and editing. For instance, the "Document" view wraps with the key names, "Xray" wraps with key names and a "<ul><li>...</li></ul>".

=[G/Z/ol/5]
