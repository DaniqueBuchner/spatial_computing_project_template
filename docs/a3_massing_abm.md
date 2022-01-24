# **The Agent Based Model**

## **Description (Objectives)**
### Criteria for the agent based model

* Value fields
    1. Noise
    2. Distance to the facade
    3. Distance to the entrance
    4. Sky view factor
* Closeness to other agents  
* Squareness of the space

### Extra function for the agent based model

* When the agent has grown to the required space needed, they can grow like a "greedy snake," such that they can compare the inner voxels with the free neighbors to choose move or not.

## **The input of the agent based model**

### Agent Preferences

Originally, all preferences range between 0 and 1. For simplification and unification of the calculating process, later a constant will be multiplied to balance the value fields.  

For the value field of **noise_field**, **dist_entrance**, and **dist_fac**, the value will time -1 since they are "the lower the better."

*(The following table is only for illustration, which is the head of the original full table)*

| space_name          | space_id | noise_field | dist_entrance | dist_fac | sunlight | skyview |
|---------------------|----------|-------------|---------------|----------|----------|---------|
| Student Housing 1 p | 0        | 0.4         | 0.55          | 0.87     | 0.8      | 0.6     |
| Student Housing 4 p | 1        | 0.4         | 0.55          | 0.87     | 0.8      | 0.6     |
| Assisted Living     | 2        | 0.8         | 0.4           | 0.93     | 0.8      | 0.8     |
| Starter Housing     | 3        | 0.6         | 0.6           | 0.93     | 0.8      | 0.6     |

### Value fields

### Adjacency matrix

