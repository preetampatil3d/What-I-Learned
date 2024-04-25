# GC
- Yong Generation (Nursery):  All the new Objects are allocated in this memory. Whenever this memory gets filled. GC is performed. This is called the Minor Garbage Collection.
- Old Generation: All long-lived objects that have survived many. Rounds of minor GC are stored in this memory. This is called a major GC
- Perm Generation / Now Metaspace:
  - Perm Gen space cannot be auto-increased. So metaspace is introduced which changes size dynamically. Also, GC is automatically triggered when class metadata usage reaches its maximum size.
  - Contains metadata required by JVM to describe classes, Methods etc
  - Metadata: Essential  information about class, class definitions, methods, fields, etc.
 
    
# Execution/ Flow

