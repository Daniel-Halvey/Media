import datetime

def calculate_edge_rank(affinity, weight, time_decay):
    """
    Calculates a simplified Edge Rank score.

    Args:
        affinity (float): A measure of the relationship strength (0 to 1).
        weight (int): Importance of the content type (1 for text, 2 for image, 3 for video).
        time_decay (float): A factor that decreases over time (0 to 1).

    Returns:
        float: The calculated Edge Rank score.
    """
    if not (0 <= affinity <= 1):
        raise ValueError("Affinity must be between 0 and 1.")
    if weight < 1:
        raise ValueError("Weight must be at least 1.")
    if not (0 <= time_decay <= 1):
        raise ValueError("Time decay must be between 0 and 1.")

    return affinity * weight * time_decay

def get_time_decay(post_time, decay_period_hours=48):
    """
    Calculates a time decay factor based on the age of the post.

    Args:
        post_time (datetime): The time the post was created.
        decay_period_hours (int): The time period (in hours) over which the score decays to 0.

    Returns:
        float: A time decay factor between 1 and 0.
    """
    now = datetime.datetime.now()
    age_in_hours = (now - post_time).total_seconds() / 3600
    return max(1 - (age_in_hours / decay_period_hours), 0)  # Decay to 0 over the specified period

# Example usage
def main():
    post1_affinity = 0.99  # Close 
    post1_weight = 1      # Image 
    post1_time = datetime.datetime.now() - datetime.timedelta(hours=2)
    post1_time_decay = get_time_decay(post1_time)

    post3_affinity = 0.99  # Acquaint
    post3_weight = 1     # Video
    post3_time = datetime.datetime.now() - datetime.timedelta(hours=10)
    post3_time_decay = get_time_decay(post2_time)

    post1_edge_rank = calculate_edge_rank(post1_affinity, post1_weight, post1_time_decay)
    post2_edge_rank = calculate_edge_rank(post2_affinity, post2_weight, post2_time_decay)

    print("Post 1 Edge Rank:", post1_edge_rank)
    print("Post 2 Edge Rank:", post2_edge_rank)

if __name__ == "__main__":
    main()
