### **`when` Condition Syntax**

The `when` directive allows tasks to be executed conditionally based on variables, facts, or expressions.

#### 1. **Basic Syntax**

```yaml
- name: Run only when condition is true
  debug:
    msg: "This will run"
  when: some_variable == "value"
```

#### 2. **Multiple Conditions**

- **AND Condition**:

```yaml
- name: Run when all conditions are true
  debug:
    msg: "All conditions are true"
  when:
    - some_variable == "value"
    - another_variable == "value2"
```

- **OR Condition**:

```yaml
- name: Run when any condition is true
  debug:
    msg: "At least one condition is true"
  when: some_variable == "value" or another_variable == "value2"
```

#### 3. **Using Facts**

```yaml
- name: Run only on specific OS
  debug:
    msg: "This is a CentOS system"
  when: ansible_facts['os_family'] == "RedHat"
```

#### 4. **Using Jinja2 Expressions**

```yaml
- name: Run if the variable exists
  debug:
    msg: "Variable is defined"
  when: my_variable is defined
```

#### 5. **Negation**

```yaml
- name: Skip if condition is not met
  debug:
    msg: "This won't run if the condition is false"
  when: not some_variable
```

---

### **`loop` Syntax**

The `loop` directive repeats a task over a list of items.

#### 1. **Basic Loop**

```yaml
- name: Loop through a list
  debug:
    msg: "Item is {{ item }}"
  loop:
    - one
    - two
    - three
```

#### 2. **Loop with Dictionaries**

```yaml
- name: Loop through dictionaries
  debug:
    msg: "Key: {{ item.key }}, Value: {{ item.value }}"
  loop:
    - { key: "name", value: "John" }
    - { key: "age", value: 30 }
```

#### 3. **Loop with `with_items` (Deprecated in Ansible 2.9)**

(Use `loop` instead of `with_items` for modern playbooks.)

```yaml
- name: Loop with items
  debug:
    msg: "Item is {{ item }}"
  with_items:
    - apple
    - banana
    - cherry
```

#### 4. **Loop with Index**

```yaml
- name: Loop with index
  debug:
    msg: "Item {{ item.0 }} is {{ item.1 }}"
  loop: "{{ items | enumerate }}"
  vars:
    items:
      - apple
      - banana
      - cherry
```

#### 5. **Nested Loops**

```yaml
- name: Nested loops
  debug:
    msg: "Outer {{ item.0 }} Inner {{ item.1 }}"
  loop: "{{ outer_list | product(inner_list) | list }}"
  vars:
    outer_list: [1, 2, 3]
    inner_list: ['a', 'b', 'c']
```

#### 6. **Loop with Register**

```yaml
- name: Loop and register results
  command: echo "{{ item }}"
  loop:
    - one
    - two
    - three
  register: loop_results

- name: Show results
  debug:
    var: loop_results.results
```

#### 7. **Break Loop Using `when`**

```yaml
- name: Loop with condition
  debug:
    msg: "Processing {{ item }}"
  loop:
    - one
    - two
    - three
  when: item != "two"
```

#### 8. **Loop Control**

- **With `loop_control` for Custom Labels**:

```yaml
- name: Customize loop output
  debug:
    msg: "Processing {{ item }}"
  loop:
    - alpha
    - beta
    - gamma
  loop_control:
    label: "{{ item | upper }}"
```

- **With `pause` Between Items**:

```yaml
- name: Pause in a loop
  pause:
    seconds: 2
  loop:
    - one
    - two
    - three
```

---

### Combining `when` and `loop`

You can combine `when` and `loop` for conditional looping.

```yaml
- name: Conditional loop
  debug:
    msg: "Item is {{ item }}"
  loop:
    - one
    - two
    - three
  when: item != "two"
```

---
### **Playbook Example: Using Variables in Loops and Conditions**

```yaml
- name: Example Playbook with Variables in Loops
  hosts: localhost
  vars:
    my_list:
      - name: John
        age: 30
      - name: Jane
        age: 25
      - name: Doe
        age: 40
    age_limit: 30
    greeting: "Hello"

  tasks:
    # Loop through my_list and print each item's details
    - name: Print user details
      debug:
        msg: "Name: {{ item.name }}, Age: {{ item.age }}"
      loop: "{{ my_list }}"

    # Loop with a condition using `when`
    - name: Print users above age limit
      debug:
        msg: "{{ greeting }}, {{ item.name }} is above {{ age_limit }}"
      loop: "{{ my_list }}"
      when: item.age > age_limit

    # Use loop with multiple variables and extra logic
    - name: Perform an action for users under the age limit
      debug:
        msg: "{{ item.name }} is under {{ age_limit }} years old."
      loop: "{{ my_list }}"
      when: item.age <= age_limit
```

---

### **Explanation**

1. **Variables in `vars` Section**:
    
    - `my_list` is a list of dictionaries containing user details (`name` and `age`).
    - `age_limit` is a variable used for the condition.
    - `greeting` is a string variable used in the task output.
2. **Using Variables in a Loop**:
    
    - The `loop` directive iterates over `my_list`. Each `item` in the loop represents a dictionary, allowing access to `item.name` and `item.age`.
3. **Conditional Execution with `when`**:
    
    - The `when` condition checks whether the `age` field in the current `item` matches the `age_limit` or other criteria.

---

### **Playbook Output**

When you run the above playbook, you will see:

1. **Task 1 Output**: Prints details for all users.
2. **Task 2 Output**: Prints greetings for users older than `30`.
3. **Task 3 Output**: Prints details for users younger or equal to `30`.

---

### **Advanced Usage**

#### Use Variables with `loop_control` for Custom Output:

```yaml
- name: Use loop_control with variables
  debug:
    msg: "Processing user {{ item.name }} (Index: {{ index + 1 }})"
  loop: "{{ my_list }}"
  loop_control:
    index_var: index
```

#### Combine Nested Loops and Variables:

```yaml
- name: Nested loops with variables
  debug:
    msg: "Outer: {{ outer_item }}, Inner: {{ inner_item.name }}"
  vars:
    outer_list: ["Group1", "Group2"]
  loop: "{{ outer_list | product(my_list) | list }}"
  loop_control:
    label: "{{ item.1.name }}"  # Label uses `my_list` items
```

This example demonstrates how to use `vars` effectively in conjunction with loops and conditions in Ansible playbooks!