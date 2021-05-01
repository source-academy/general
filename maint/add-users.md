# Adding users

## Using a CSV

Note that this method is mainly for bulk-importing student and staff users. This will let you assign group leaders as well.

1. Prepare the following CSVs, without headers and with the fields name, username, group name, in that order. All fields are required.

   1. students
   2. group leaders (TAs)
   3. mentors

   You only need to create the CSVs that you wish to update.

   Users are keyed by username, and groups are keyed by group name. If there are multiple rows with the same key, the later rows will overwrite the earlier rows.

   Leaders and mentors are created with the staff role. They are set as the leader (or mentor) of the specified group, but they are not themselves added to the group.

   Note: groups are used only to group students into their tutorial groups. The group leader is meant to be the group's TA, and the leader's grading view will be by default filtered to show only their group's submissions. The group mentor field is currently unused.

2. Transfer the CSVs to the server the backend is running on. Note: if you are using our Terraform deployment with 2 (or
   more) redundant nodes, you only need to transfer the CSVs (and do the remaining steps) on one of the nodes.

3. Enter the Elixir REPL in the context of the backend. If you followed the deployment guides, you can run `/opt/cadet/bin/cadet remote` to do so.

4. Execute `Mix.Tasks.Cadet.Users.Import.run(nil)`

5. Enter the paths to the files as needed.

6. Done!

## Directly via SQL

1. Connect to your PostgreSQL database using `psql` or whatever preferred tool you use.

2. Add users:

   ```sql
   INSERT INTO users (name, role, username, inserted_at, updated_at) values ('name', 'admin', 'username', NOW(), NOW());
   ```

3. Done!
