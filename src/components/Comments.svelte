<script>
    export let comment;

    function addComment() {

    }
</script>

<style>
    .card .card-content .card-title {
        margin-bottom: 0;
    }

    .card .card-content p.timestamp {
        color: #999;
        margin-bottom: 10px;
    }

    h3 {
        text-align: center;
    }
</style>

<div class="row">
    <div class="card">
        <div class="card-content">
            <p class="card-title">{comment.testo}</p>
            <p class="timestamp">{comment.data}</p>
            <p>Post created by: {comment.persona.username}</p>
        </div>
    </div>
</div>