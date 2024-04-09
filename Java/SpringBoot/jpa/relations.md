# Relations
## @OneToOne
- OneToOne Bi-directional
- Add @oneToOne on both entities, id column of each will created in the opposite table - Not recommended
- One Entity should own the relationship, So add the attribute mappedBy=tableName to the opposite Entity.
- The default fetch type is eager, It may result in an infinite loop, So add Lazy to one of the relations
```
@Entity
Public Class Passport{
	@OneToOne( fetch=FetchType.Lazy,  mappedBy=“passport”)
	private Student;
}
@Entity
Public Class Student{
	@OneToOne
	private Passport; // owns the relation

	// passport_id will be created as we added the mappedBy attribute in Passport
}
```

## @OneToMany @ManyToOne
```
@Entity
Public class Project{
	// // Adding mappedBy here, will generate a reference id in the opposite table
	@OnwToMany(mappeBy = “project”) // <————- 
	private List<Task> tasks;
}

@Entity
Public class Task{
	@ManyToOne
	@JoinColumn(name = “projectId”)
	private Project project;
}

```
- Prevent parent removal when relevant child exist
```
// at Parent Entity
@PreRemoval
Public void preremoveValidation(){
	if(!this.childVariable.isEmpty()){
		throw new RunTimeException(‘can’t remove  parent when child exist’);
	}
}

```

### CascadeType.ALL, Remove, Persist: Usually attribute added to parent oneToMany
- When we perform PERSIST/MERGE/REMOVE operation on the parent, the same operation is executed on the child
  - ALL: Apply all Cascading options
  - Remove: Delete child entities when the parent entity is removed
  - Persist: When we add some child entity in parent Object and save parent. Child also get saved

###  OrphanRemoval
- If we want to remove only child records, not parent
- Add ‘orphanRemoval = true ‘  attribute in Parent (OneToMany)
```
@OneToMany(cascade = CascadeType.ALL, mappedBy = ‘project’, orphanRemoval = true)
Private List<Task> tasks;
```

## @ManyToMany
- It will create a third table with join of two tables
- unidirectional: If we can @ManyToMany for User
- If we use manyToMany on both side, It will create two relation_table, Which is data inconsistency.

 **uni-directional**
```
@Entity
Public class User{
	@ManyToMany
	@JoinTable(name=‘thirdTable’, // Not required, But want to customise we can use
		joinColumn= {
			@joinColumn(name=‘userId’)
		},
		inverseJoinColumn= {
			@JoinColumn(name=‘project_id’)
		}
	)
	private List<Project> projects;
}

@Entity
Public class Project{
	@ManyToMany
	private List<User> users;

}
// Third table will be created : user_project - user_id, project_id
```

**Bi-Directional**
```
@Entity
Public class User{
	@ManyToMany
	private List<Project> projects;
}

@Entity
Public class Project{
	@ManyToMany(mappedBy = “user”) // mappedBy will prevent creating new relation table and mapped by user
	private List<User> users;

}
// Third table will be created : user_project - user_id, project_id
```
