const express = require('express');
const router = express.Router();
const Project = require('/models/Project');
router.get('/', async (req, res) => {
    try {
        const projects = await Project.find();
        res.json(projects);
    } catch (err) {
        res.status(500).send('Server error');
    }
});
router.post('/',async (req, res) => {
    const { title, description, technologies, githubLink, liveDemoLink } = req.body;
        try {
        const newProject = new Project({
            title,
            description,
            technologies,
            githubLink,
            liveDemoLink,
        });
        const project = await newProject.save();
        res.json(project);
    } catch (err) {
        res.status(500).send('Server error');
    }
});
router.delete('/:id', async (req, res) => {
    try {
        const project = await Project.findById(req.params.id);
        if (!project) return res.status(404).json({ msg: 'Project not found' });
        await project.remove();
        res.json({ msg: 'Project removed' });
    } catch (err) {
        res.status(500).send('Server error');
    }
});
module.exports = router;
