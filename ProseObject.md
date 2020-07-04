# Stage1 - validate the basic document rendering concept:

Narrative version of the core paradigm (currently PoC-ed in perl, and version with edit-in-place interface and navigation shown at https://www.youtube.com/watch?v=4ZfsyTPYFIA):

1. A Tree of Records

   Given a tree of Records based at a "Root" where each Record is:

   1.  a hash of key/values and

   1.  a list of other Records referenced with/without Prefixes


1. Record Features

   Assuming

   1. Each Record has a unique name

   1. The keys are more or less any string, case-sensitive, without leading or trailing whitespace

   1. The values are strings and may contain {Entities} and HTML markup.

1. Rendering

   1. Given an input of:

      1. the name of a Root1
   
      2. the name of a Record1

      3. the name of a Key1

      4. a desired View

   2. Return a string which is:
   
      1. The Value of the Key1, recursively expanding any {Entities}

      2. By replacing the {Entities} with the value of a matching Key


   3. Matching an Entity with a Key

      1. General

         1. Search for the Key in Record1, or in a linked Record, inquiring depth-first

         2. The search for a match always starts at the top, from Record1. (Enabling modification by overides.)

      2. Prefixing:

         1. Rule: If the {Entitiy} is in a Prefixed Record, then treat the {Entity} as being {PrefixEntity}. E.g. if the Record2 is referenced as Buyer.=[Record2], then {Name} in a value in Record2 is treated as {Buyer.Name}. Recursively. So if {Name} is in Record3 and Record2 references it as President.=[Record3], then {Name} is treated as {Buyer.President.Name}.

         2. Deprefixing: If there is no match for {Buyer.President.Name}, then look for {Buyer.Name} and then for {Name}. (Note - this deprefixing has nothing to do with the periods, it works the same for {BuyerPresidentName}, if the references are Buyer=[Record2], President=[Record3]. The periods are characters like any other.) (Note also that the deprefixing starts from the rightmost Prefix.)

      3. Failure to Find a Match

         1. Leave any unmatched {Entities} in the string (optionally marking them in red).
         
   4. View: Metadata Wrapping Each Substitution

      1. Depending on the View, wrap each substitution with metadata about its source (Record and Key) to aid navigation and editing. For instance, the "Document" view wraps with the key names, "Xray" wraps with key names and a "&lt;ul>&lt;li>...&lt;/li>&lt;/ul>".

