<script>
    export let data;
</script>

<article class="card">
    <header class="header">
        <img class="avatar" src={data.avatar} alt="profile avatar" />

        <div class="info">
            <h2 class="name">{data.name}</h2>
            <p class="headline">{data.headline}</p>
            <p class="location">{data.location}</p>
        </div>
    </header>

    <section class="skills">
        {#each data.skills as skill}
            <span class="chip">{skill}</span>
        {/each}
    </section>

    <section class="bottom-grid">
        <div class="left-col">
            <div class="about">{data.about}</div>
        </div>

        <div class="right-col">
            <div class="meta">
                <div class="metric">
                    <span class="value">{data.connections}</span>
                    <span class="label">Connections</span>
                </div>
            </div>

            <div class="footer">
                <a href={data.profileUrl} target="_blank" rel="noopener" class="btn">
                    View LinkedIn →
                </a>
            </div>
        </div>
    </section>


</article>

<style>
.card {
    background: var(--color-sand);
    /* background-image: var(--logo-linkedin); */
    color: var(--color-ink);
    padding: 1.3rem 1.5rem;
    border-radius: var(--radius-lg);
    border: 1px solid var(--color-apricot-border);
    max-width: 420px;
    box-shadow: var(--shadow-soft), var(--shadow-warm);
    transition: transform 0.18s ease, box-shadow 0.18s ease;
}

.header {
    display: flex;
    gap: 1rem;
    align-items: center;
    margin-bottom: 0.5rem;
}

.avatar {
    width: 64px;
    height: 64px;
    border-radius: 50%;
    object-fit: cover;
    border: 3px solid var(--color-apricot);
    box-shadow: 0 0 8px var(--color-apricot-glow);
}

.name {
    margin: 0;
    font-size: var(--fs-lg);
    font-weight: 600;
}

.headline,
.location {
    margin: 0;
    font-size: var(--fs-sm);
    color: var(--color-ink-light);
}

.skills {
    margin-top: 0.75rem;
    display: flex;
    flex-wrap: wrap;
    gap: 0.4rem;
}

.chip {
    padding: 0.25rem 0.65rem;
    border-radius: 999px;
    background: var(--color-lavender);
    color: var(--color-ink);
    border: 1px solid var(--color-chip-border);
    font-size: var(--fs-xs);
    font-weight: 500;
}

.bottom-grid {
    margin-top: 1rem;
    display: grid;
    grid-template-columns: 1fr auto;
    grid-column-gap: 1.2rem;
    align-items: start;
}

.left-col {
    max-width: 260px;
}

.about {
    font-size: var(--fs-sm);
    line-height: 1.45;
    color: var(--color-ink-light);
}

.right-col {
    display: flex;
    flex-direction: column;
    gap: 0.9rem;
    align-items: flex-start;
    width: fit-content;
}

.meta {
    width: fit-content;
}

.metric {
    text-align: left;
}
.value {
    font-size: var(--fs-md);
    font-weight: 600;
    color: var(--color-tea);
}

.label {
    font-size: var(--fs-xs);
    color: var(--color-ink-lighter);
}

.footer {
    width: fit-content;
}

.btn {
    background: var(--color-apricot);
    padding: 0.5rem 0.85rem;
    border-radius: var(--radius-sm);
    font-size: var(--fs-sm);
    color: var(--color-ink);
    font-weight: 600;
    text-decoration: none;
    white-space: nowrap;
    transition: background 0.2s ease, transform 0.15s ease;
}

.btn:hover {
    background: var(--color-apricot-dark);
    transform: translateY(-1px);
}
</style>
