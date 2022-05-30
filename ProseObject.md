# Stage1 - validate the basic document rendering concept:

Narrative version of the core paradigm (currently PoC-ed in perl, and version with edit-in-place interface and navigation shown at https://www.youtube.com/watch?v=4ZfsyTPYFIA):

1. A Tree of Records

   Given a tree of Records based at a "Root" where each Record is:

   1.  a hash of key/values and

   1.  a list of other Records referenced with/without Prefixes (the syntax is OptionalPrefix=[OtherRecord])


1. Record Features

   Assuming

   1. Each Record has a unique name

   1. The keys are more or less any string, case-sensitive, without leading or trailing whitespace (syntax is Key Name=ValueAsAString)

   1. The values are strings and may contain {Entities} and &lt;HTML&gt; markup.

1. Rendering

   1. Given an input of:

      1. the name of a Root1
   
      2. the name of a Record1

      3. the name of a Key1

      4. a desired View

   2. Return a string which is:
   
      1. The Value of the Key1, recursively expanding any {Entities}

      2. By replacing the {Entities} with the Value of _the first_ matching Key


   3. Matching an Entity with a Key

      1. General

         1. Search for the Key in Record1, or in a linked Record, inquiring depth-first

         2. The search for a match always starts at the top, from Record1. (Enabling modification by overides.)

      2. Prefixing:

         1. Linked Records: When searching a Prefixed linked Record (Buyer.=[AcmeInc.md]) treat each of the KeyNames in the linked Record (AcmeInc.md) as if the KeyName was Prefixed (e.g. Name=Acme is treated as Buyer.Name=Acme).

         2. Entities in Prefixed Records: If an {Entity} is in a Prefixed Record, then treat the {Entity} as being {PrefixEntity}. E.g. if we are in Record1, look for {Buyer.Name}, don't find Buyer.Name=Something in Record1, then look in AcmeInc.md, we might find Name=Acme. In which case the match is made.  If we look for {Buyer.President.Name} in Record1, then AcmeInc.md and find a link there to President.=[JonesJ.md] then a key Name=Jones would satisfy.  If in JonesJ.md Name={First.Name} then we would have a match, but would then be looking for {Buyer.President.Name.First} again starting from Record1, then {President.Name.First} in AcmeInc.md, then (if not found) for Name.First in JonesJ.md.  If still not found then deprefixing would occur. 

         3. Deprefixing: If there is no match for {Buyer.President.Name.First}, then look for {Buyer.Name.First} and then for {Name.First}. (Note - this deprefixing has nothing to do with the periods, it works the same for {BuyerPresidentName}, if the references are Buyer=[Record2], President=[Record3]. The periods are characters like any other.) (Note that the deprefixing starts from the rightmost Prefix and peels them off from the right recursively.). (Note also that this example of Buyer.President.Name.First ends up looking for some non-sensicle matches, including Name.First in the AcmeInc.md Record and in Record1, but this is still the needed behavior.)

      3. Failure to Find a Match

         1. Leave any unmatched {Entities} in the string (optionally marking them in red).
         
   4. View: Metadata Wrapping Each Substitution

      1. Depending on the View, wrap each substitution with metadata about its source (Record and Key) to aid navigation and editing. For instance, the "Document" view wraps with the key names, "Xray" wraps with key names and a "&lt;ul>&lt;li>...&lt;/li>&lt;/ul>".


