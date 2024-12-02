### Step 1: List Compartment IDs
Use the following command to list all compartments:

```bash
oci iam compartment list --compartment-id <root_compartment_ocid> --query "data[*].{ID:id, Name:name}" --output table
```

**Explanation:**
- `--compartment-id <root_compartment_ocid>`: Replace `<root_compartment_ocid>` with the OCID of the root compartment (or the parent compartment) from which you want to start listing. If you want to list all compartments from the root compartment, use the root compartment OCID.
- `--query`: Filters the output to display only the compartment ID and name.
- `--output table`: Formats the output as a table for better readability.

### Example Output:
```
+--------------------------------------+--------------------------+
| ID                                   | Name                     |
+--------------------------------------+--------------------------+
| ocid1.compartment.oc1..examplecompid | example-compartment-1     |
| ocid1.compartment.oc1..anothercompid | example-compartment-2     |
+--------------------------------------+--------------------------+
```

### Step 2: List All Compartment IDs Across Your Organization
If you want to list compartments from all regions and compartments, ensure you have the right permissions to access those resources and replace `<root_compartment_ocid>` accordingly.