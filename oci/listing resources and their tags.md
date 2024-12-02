### Step 1: List Resources and Their Tags

#### 1.1 Compute Instances
To list compute instances and their tags, you can use the following command:

```bash
oci compute instance list --compartment-id <compartment_ocid> --query "data[*].{ID:id, Name:display-name, Tags:freeform-tags}" --output table
```

**Explanation:**
- `--compartment-id <compartment_ocid>`: Replace `<compartment_ocid>` with the OCID of the compartment where the resources are located.
- `--query`: Filters the output to show only the instance ID, name, and associated tags.
- `--output table`: Formats the output as a table.

#### 1.2 Other Resource Instances (Example for VCN)
To list resources like VCNs and their tags, you can use:

```bash
oci core vcn list --compartment-id <compartment_ocid> --query "data[*].{ID:id, Name:display-name, Tags:freeform-tags}" --output table
```

### Step 2: List Other Types of Resources
The same command can be adapted for other types of resources, such as volumes, databases, etc. Here are some examples:

- **List Volumes**:
  ```bash
  oci core volume list --compartment-id <compartment_ocid> --query "data[*].{ID:id, Name:display-name, Tags:freeform-tags}" --output table
  ```

- **List Databases**:
  ```bash
  oci database db-system list --compartment-id <compartment_ocid> --query "data[*].{ID:id, Name:display-name, Tags:freeform-tags}" --output table
  ```

### Step 3: Format and Display Information
By using the `--output table` flag, the output will be formatted as a table in the terminal. Here’s an example of how it will look:

```
+--------------------------------------+----------------------+-----------------------------+
| ID                                   | Name                 | Tags                        |
+--------------------------------------+----------------------+-----------------------------+
| ocid1.instance.oc1..xxxxxxxxxx       | ExampleInstance      | {"Environment": "Development"} |
| ocid1.instance.oc1..xxxxxxxxxx       | ProductionInstance   | {"Environment": "Production"} |
+--------------------------------------+----------------------+-----------------------------+
```

### Step 4: Tables for Multiple Resource Types
If you want to list tags for multiple resource types in a single table or script, you can group the commands like this example:

```bash
oci compute instance list --compartment-id <compartment_ocid> --query "data[*].{ID:id, Name:display-name, Tags:freeform-tags}" --output table
oci core vcn list --compartment-id <compartment_ocid> --query "data[*].{ID:id, Name:display-name, Tags:freeform-tags}" --output table
oci core volume list --compartment-id <compartment_ocid> --query "data[*].{ID:id, Name:display-name, Tags:freeform-tags}" --output table
```

### Step 5: Filtering and Further Customization
If you want to filter or customize the query for other types of tags or resources, you can adjust the `--query` parameter accordingly. For example, if you want to show only a specific tag key:

```bash
oci compute instance list --compartment-id <compartment_ocid> --query "data[*].{ID:id, Name:display-name, Tags:freeform-tags.Environment}" --output table
```

This will display only the "Environment" tag instead of all tags.

### Conclusion
By using the `oci` command with the `--query` and `--output table` options, you can list resources and their tags in an organized way.